---
name: minitool-h5-builder
description: >-
  开发或改写小红书小工具的离线 H5。根据任务按需召回官方容器规则、平台 JS API、构建与性能专题，并产出可核验的小工具 ZIP。适用于小工具首屏、资源、WebGL、Worker 和客户端兼容性问题。
compatibility: MiniTool CLI with docs commands for platform knowledge; local build and ZIP tools for artifact delivery.
metadata:
  version: "0.2.4-beta.1"
---

# 小红书小工具 H5 构建

先确定用户要交付 ZIP、适配现有 H5，还是只核对某项能力。用户明确指定 Skill 名称与版本时，按指定版本执行。改写已有页面时记录原有核心流程，交付时用相同输入核对其行为。

## 开始任务

1. 开发或改写时先读 [离线基线](references/offline-baseline.md)，确认容器兼容性和断网边界。涉及 ZIP 交付时，在确定输出结构前读 [ZIP 打包与验收](references/zip-artifact-spec.md)。单项咨询直接选择对应专题。
2. 按 [召回指引](references/recall.md) 获取当前专题目录，再选择匹配任务的条目批量读取官方章节。编写交付代码时核对兼容性和资源加载规则。迁移站点时读取 `h5.rewrite` 和 `restrictions`。涉及平台 JS API 时读取调用约定、具体 API 及客户端版本要求。
3. 新建或改写入口时，从 `index.html` 设计 JS 执行前能看到的内容。涉及加载态、分阶段启动或首屏优化时，加载 `perf.startup`。按文档明确的客户端、环境和权限条件选实现或降级路径。
4. 交付 ZIP 前按打包规范核验最终目录和 ZIP，记录路径、包体和实际校验结果。使用同一 ZIP 在创服模拟器及 Android、iOS 真机验证，交付时明确已验证和待验证项。

## 按任务加载

| 场景 | 优先读取 |
| --- | --- |
| 生成 ZIP、核对入口或资源路径 | [ZIP 打包与验收](references/zip-artifact-spec.md)，涉及工程构建时召回对应专题 |
| 首屏空白、启动 Loading、复杂游戏启动 | 首屏与分阶段启动，按需加读指标与验收 |
| 大图、字体、平台托管、OSS | 资源加载规则、平台静态资源优化与体积预算 |
| webpack、Vite、三方组件或引擎 | `perf.build`、`perf.frameworks`，交付时加读 `h5.zip` |
| 触摸、滚动、宿主按钮与视口适配 | 页面与渲染，按需读取窗口信息 API |
| WebGL 或 Worker | `perf.graphics`、`worker`，按需读取 `perf.budget` |
| 全屏笔记预览与进入体验 | `fullscreen-note`、`api.window`，需要分享时加读 `api.share` |
| 模拟器、创服工作台、查询、上传或提审 | [现有 CLI 指引](references/cli.md) |

公开说明用于判断接入条件。受控能力先由平台校验项目、身份和环境，通过后才按获准的说明调用已注册工具。保留基础交互作为降级路径。上传、预览和提审沿用用户已授权的动作范围。

文档不可用时按随包基线完成通用静态 H5 工作。将未能在线确认的章节和 API 明确标出，使用基础实现或降级。已核验的兼容快照在同一任务内可以继续使用。
