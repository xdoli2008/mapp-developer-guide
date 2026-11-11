---
title: "mapp-developer-guide — Dev Workflow"
type: recipe
tags: [workflow, dev-workflow, vite, auto-generated]
created: 2025-11-11T16:30:00+08:00
source: spec-setup
---

# mapp-developer-guide — Dev Workflow

## Goal
本地启动 demo 开发服务器（Vite HMR），在浏览器中迭代开发。

## Prerequisites
- Bun >= 1.4.3；依赖已 `bun install`

## Steps
1. 进入目标 demo 目录：`cd demos/brick-breaker`
2. 启动 dev server：`bun run dev`（即 `bun --bun vite`）
3. 打开对应端口（127.0.0.1，strictPort，固定不冲突）：
   - brick-breaker → http://127.0.0.1:5191
   - game-2048 → http://127.0.0.1:5192
   - capability-lab → http://127.0.0.1:5193

## Expected Outcome
Vite dev server 起在对应端口并带 HMR；`main.ts` 在 DEV 模式下传 `null` 给 `createHost`（不依赖真实平台宿主，走本地 stub）。

## Common Pitfalls
- 端口 strictPort：被占用直接失败，不会自动换端口；三个 demo 并行开发时注意按上表对号
- DEV 模式下 SDK 相关路径（用户身份、主题等）是 host stub 行为，与沙箱内真实宿主有差异——最终验收以 `build:debug` 上传沙箱为准
- `src/template.html` 改动经 `demo-body` 插件注入，需刷新确认（dev 与 build 的注入时机一致）

## Related
- [[build-workflow]] / [[debug-workflow]] — 配套流程
