# 按需查询平台规范

先确认目标客户端和最低版本，再选择实际要实现的专题。通过用户安装的 MiniTool CLI 查询结构化资料，不在 Skill 中配置服务地址、文档站路径或联调环境。

## 查询步骤

```sh
redminitool docs topics --json
redminitool docs get h5.rewrite h5.compat resource-rules h5.zip --json
redminitool docs get api.call api.client-version api.save-image --json
```

使用当前版本的 `--help` 核实命令。已知专题时直接 `docs get`；不清楚 API 时先读 `api.index`，再核对签名、版本、权限、错误码和降级。一次查询当前任务需要的章节，碰到新需求时再加读。

若响应提供目录修订号、正文修订号和 ETag，记录实际使用的版本；同一任务内复用已读正文，新任务重新查询。请求失败时明确记录失败，不把未验证的 API 当作可用能力。

## 查询不可用

没有兼容 CLI 或查询失败时，按 [离线基线](offline-baseline.md) 和 [ZIP 打包与验收](zip-artifact-spec.md) 完成基础静态 H5 工作。平台 API 和兼容条件保留为待核验项，使用基础实现或降级；不要猜测服务地址或绕过访问条件。

## 平台操作

实际使用平台能力前，核对项目、身份和权限。调用以平台实时返回为准，无法核验时保留基础路径。上传、预览和提审只执行用户已经授权的动作。
