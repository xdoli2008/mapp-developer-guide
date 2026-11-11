# Project: mapp-developer-guide

## What This Is

Discourse 社区「微应用」（Mini-App）平台的开发者指南仓库。为一个构建在 Discourse 论坛之上、让第三方开发者在社区内发布沙箱化小游戏/小工具的开放平台，提供完整的中文开发文档（docs/）、官方 SDK（`@discourse-mini-app/sdk`）和三个可运行示例工程（demos/）。它是面向平台开发者的配套交付物，类似微信小程序的「开发文档 + SDK + demo」三件套。

## Core Value

**让第三方开发者仅凭本仓库（文档 + SDK + 示例）就能独立完成一个微应用的构思、开发、调试、打包、提审到上线的全流程。** 文档与 SDK/平台实际行为的一致性是这一价值成立的根基。

## Requirements

### Validated

<!-- Shipped and confirmed valuable. -->

- [x] 13 篇中文开发文档（约 1400 行），覆盖发布流程、manifest 格式、限额、CDN 素材、WASM、音视频、网络鉴权、调试等全链路
- [x] `@discourse-mini-app/sdk` v0.3.0：6 个导出子模块（运行时 / pack 打包 / server 验签 / network / media / timeline），含完整 TypeScript 类型声明与约 30 项细粒度权限封装
- [x] 3 个完整示例工程（Bun 1.4.3+ / Vite 8.3.5 / TypeScript）：brick-breaker（最小模式）、game-2048（基础权限组合）、capability-lab（18 项权限全覆盖能力测试台），均自带 bun test / typecheck / build / build:debug

### Active

<!-- Current scope being built toward. These are hypotheses until shipped. -->

- [ ] 想法孵化：新微应用的 idea 阶段在本仓库 brainstorm 探索与验证，成熟后经模板派生独立仓库（见 NEW-APP.md；brainstorm 产物落在 .workflow/scratch/，不入库）
- [ ] 文档与平台演进保持同步：平台新增能力时补充对应文档章节，行为变更时修订既有描述
- [ ] 文档错误与过时内容修正：限额表、错误码、API 签名等关键事实与 SDK 实际行为核对一致
- [ ] 文档内示例代码与 demos/ 实际工程保持一致（复制可用、不误导）

### Out of Scope

<!-- Explicit boundaries. Include reasoning to prevent re-adding. -->

- 微应用平台本体（宿主 Discourse 插件/服务端实现）— 不在本仓库中，本仓库只是配套交付物，平台行为以平台实现为准
- SDK 运行时桥接层源码重写 — sdk/ 中 .mjs 为平台压缩产物，按发布物对待而非可重构源码
- 示例工程的大规模架构改造 — 示例的价值在于最小化展示能力用法，不追求工程化完备
- idea 成品开发 — 成熟想法必须转移到独立仓库开发，本仓库只承载孵化（避免模板污染与职责混杂）

## Context

- 平台形态：用户在社区个人资料页创建应用 → 用本仓库 SDK 开发 → 打包 app.zip 上传沙箱 → 提审 → 管理员审核后上线；应用以受控 iframe 嵌入论坛页面运行
- 安全模型：约 30 项细粒度权限声明 + 网络端点/外链/CDN 素材 manifest 白名单 + CDN 素材 SHA-256 钉死校验 + 人工审核；应用访问不到论坛 Cookie / 父 DOM / 其他应用数据
- 构建模型：Vite 打出单 IIFE app.js + app.css，index.html 为正文片段（禁内联脚本/样式）；必须用 SDK pack 打包（路径规范化、UTF-8 ZIP、越界/重复路径拒绝）
- 限额：解压 128 MiB / 单文件 32 MiB / 1024 文件；大素材走获批 CDN
- 审核链路：沙箱构建 7 天过期、同 SHA-256 重复上传拒绝（BUILD_EXISTS）、正式版只能升版、可回滚
- Git 历史较新（13 次提交，均为 "update"），说明仓库处于早期同步阶段；平台本体成熟度高于本仓库文档成熟度

## Constraints

- **兼容性**: SDK 公开 API（35 个命名空间）与 manifest 格式是开发者依赖的契约 — 修订文档涉及 API 描述时必须与 sdk/index.d.ts 实际签名核对，不得凭记忆撰写
- **语言**: 全部文档为简体中文，技术术语保留英文 — 保持现有文风一致
- **工具链**: 示例工程锁定 Bun 1.4.3+ / Vite 8.3.5 / TypeScript 5.9.x，通过 file:../../sdk 引用本地 SDK — 文档中的命令示例必须与该工具链匹配
- **平台权威**: 平台本体不在本仓库，文档描述与平台行为冲突时以平台为准 — 发现冲突属于需要上报/跟进的事项，而非单方面改文档迁就

## Tech Stack

- **Language**: TypeScript / JavaScript（文档示例）、Markdown（文档主体）
- **Framework**: Vite 8.3.5（构建）、Bun 1.4.3+（运行时/测试/打包脚本）
- **Database**: 无（纯前端沙箱应用 + 文档仓库，无持久层）

## Key Decisions

<!-- Decisions that constrain future work. Add throughout project lifecycle. -->

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| 仓库定位为文档维护为主 | 用户确认（2025-11-11 init 访谈）；平台演进快，文档同步是当前最大价值点 | — Pending |
| 仓库设为 GitHub Template Repository（整仓快照形态） | 用户确认（2025-11-11）；后续多个微应用项目均基于本仓库模板创建独立 repo；起步清单见 NEW-APP.md | 已生效（is_template=true） |
| 本仓库兼任想法孵化器 | 用户确认（2025-11-11）；idea 阶段在此 brainstorm 验证（复用平台知识库），成熟后模板派生独立仓库；孵化产物仅存 .workflow/scratch/（gitignore），不进 master 以保模板纯净 | 已生效 |
| 示例作为文档事实来源（source of truth）之一 | 三个 demo 覆盖最小→全能力谱系，文档示例代码应与 demo 互相印证 | — Pending |

## Stakeholders

- 第三方微应用开发者（文档的最终读者）
- 平台维护方（文档内容的上游权威）
- 社区管理员（审核流程文档的使用者）

---
*Last updated: 2025-11-11 after initialization*
