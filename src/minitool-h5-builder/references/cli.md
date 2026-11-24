# MiniTool CLI 接入

小工具命令入口是用户安装的 `redminitool`。使用已安装版本的 `--help` 核实可用命令和参数。

| 需求 | 入口 |
| --- | --- |
| 本地模拟器 | `redminitool dev --dir ./dist` |
| 创服工作台 | `redminitool inspect --no-open --json`，从 stderr 的 `INSPECT_READY` 取工作台地址 |
| 查询能力配置或版本 | `redminitool config`、`redminitool status --app-id <id>`、`redminitool history --app-id <id>` |
| 校验上传命令 | `redminitool upload --file ./app.zip --type pkg --dry-run --json` |
| 校验预览或提审参数 | `redminitool preview --file ./preview.json --dry-run`、`redminitool submit --file ./submit.json --dry-run` |

用户只要求生成 ZIP 时，在本地完成产物与校验即可。平台操作使用目标环境与已授权的账号，实际上传、创建预览或送审前核对用户授权和产物。项目内配置及外部能力的实际权限以平台回读为准。

在线章节使用 [召回命令](recall.md)。`docs get --json` 回显实际正文修订号，可作为交付引用。

Skill 在独立的 `minitool-public-skill` 仓库维护，通过 Vercel Skills 的 Git 链路独立更新。使用支持 Git 下发的 CLI 时，运行 `redminitool skill install --source https://github.com/red-minitool/minitool-public-skill.git#beta --agent codex --json` 安装，再用 `redminitool skill check --json` 检查、`redminitool skill update --json` 更新。Git 来源、ref、目录和内容摘要由 Vercel Skills 写入 `skills-lock.json`；已有安装沿用该记录，无需为 Skill 内容更新重新发布 CLI。

安装时可用 `--source <git-url>#<ref>` 指定可访问的 Git 来源及分支或标签，也可设置 `REDMINITOOL_SKILL_SOURCE`。CLI 包的安装来源不控制 Skill 的 Git 来源。

`setup` 从当前 CLI 内置副本完成离线初始化。后续需要 Git 更新时，先用 `skill install` 接管一次；仅接管 Setup 创建且内容未修改的副本，手动副本和本地修改会停止安装。Git 托管副本继续用 `skill check/update`，Setup 不覆盖它。
