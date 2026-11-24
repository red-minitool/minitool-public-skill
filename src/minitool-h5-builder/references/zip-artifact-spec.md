# ZIP 打包与验收

先用项目现有构建链得到干净的静态输出目录。原生 HTML/CSS/JS 可以直接整理到专用目录。使用 webpack、Vite 或引擎时，通过 `perf.build` 召回构建与分片建议，以下检查始终以最终静态产物为对象。

## 1. 整理目录

- 包内只保留一个 HTML 文件，放在 ZIP 根目录并命名为 `index.html`。其他文件可以平铺或分目录。压缩输出目录里的内容，若归档路径为 `dist/index.html`，容器将找不到入口。
- 产物文件按当前资源加载规则核对，通过 [召回指引](recall.md) 获取 `resource-rules`。离线清单为 `.html`、`.css`、`.js`、`.png`、`.jpg`、`.jpeg`、`.gif`、`.webp`、`.svg`、`.woff`、`.woff2`、`.json`。排除源码、构建配置、`.map`、`node_modules/` 和 `.git/`。
- 核对 HTML、CSS、JS 与延迟分片引用的文件都在包内，大小写一致，用相对路径指向实际文件。入口采用包内经典脚本，去掉 `<base>`、根绝对路径和外域资源引用。平台外移仍使用包内相对路径，资源随完整 ZIP 上传。
- 在构建后的 `index.html` 检查 viewport、静态首屏及脚本加载顺序。JS、CSS 兼容 Chrome 61，依赖产物和按需分片也要检查。涉及启动 Loading 或游戏启动时按 `perf.startup` 召回示例。

## 2. 生成 ZIP

从项目根目录执行，将 `dist` 替换成实际输出目录，`tool.zip` 使用一个尚不存在的路径。每次生成新 ZIP 可避免遗留已经删除的资源。

```sh
test -f ./dist/index.html && test ! -e ./tool.zip && (
  cd ./dist && zip -r ../tool.zip . -x '.DS_Store' '*/.DS_Store'
)
```

## 3. 验收交付

- 有 MiniTool CLI 时，运行 `redminitool upload --file ./tool.zip --type pkg --dry-run --json`。该命令执行本地 ZIP 结构、体积与完整性校验，返回摘要，无需上传。可用参数以安装版本的帮助为准。
- 无 CLI 时，用 `wc -c < ./tool.zip` 查压缩包字节数，用 `unzip -l ./tool.zip` 查解压总量和条目数。大小合规后运行 `unzip -t ./tool.zip` 检查完整性。用 `unzip -Z1 ./tool.zip` 核对文件清单，确认根目录 `index.html` 只出现一次，路径无重复、绝对路径或 `..`。用 `unzip -Z -l ./tool.zip` 确认无符号链接。也可使用系统压缩工具执行等效检查。
- MiniTool CLI 当前接受非空、最多 10 MiB 的 ZIP，解压最多 30 MiB、最多 2000 个条目。性能预算建议 ZIP 控制在 2 MiB 内。单个 HTML、CSS、JS、JSON 超过 2 MiB 或解压文本合计超过 5 MiB 时，检查大数据与解析成本。单条 Base64 解码后超过 100 KiB 时优先拆成包内文件，超过 1 MiB 须拆分。WebGL 等场景预算按 `perf.graphics`、`perf.metrics` 查阅。
- 静态检查后，核验资源路径、脚本执行与核心交互。普通浏览器可做初步冒烟。同一个 ZIP 的容器验收以创服模拟器、Android 与 iOS 真机为准，交付时记录已验证项和待验证项。

官方工程构建与产物治理按 [召回指引](recall.md) 获取 `perf.build`、`h5.zip`。
