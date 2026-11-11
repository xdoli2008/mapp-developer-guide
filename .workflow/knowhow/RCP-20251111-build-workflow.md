---
title: "mapp-developer-guide — Build Workflow"
type: recipe
tags: [workflow, build-workflow, vite, auto-generated]
created: 2025-11-11T16:30:00+08:00
source: spec-setup
---

# mapp-developer-guide — Build Workflow

## Goal
从 demo 源码产出可提审的 `app.zip`（typecheck → Vite 构建 → SDK pack 打包）。

## Prerequisites
- Bun >= 1.4.3；Vite 8.3.5、TypeScript 5.9.3（devDependencies，`bun install` 安装）
- 本地 SDK 经 `file:../../sdk` 可解析

## Steps
1. 进入目标 demo 目录：`cd demos/brick-breaker`
2. 运行完整构建：`bun run build`（等价于依次执行：
   1. `tsc -p tsconfig.json`（typecheck，strict）
   2. `bun --bun vite build`（单 IIFE 产物 → `dist/web/app.js` + `app.css`）
   3. `bun run scripts/package.ts`（注入 template.html、收集固定文件、校验 manifest.assets 路径、`pack()` 生成 ZIP））
3. 调试版（带 inline source map 的沙箱调试包）：`bun run build:debug`（即 `MINI_APP_DEBUG=1 bun run build`）
4. 产物位置：网页产物在 `dist/web/`，最终包在 `dist/package/app.zip`

## Expected Outcome
typecheck 零错误；`dist/package/app.zip` 生成。可用 `MINI_APP_ID` / `MINI_APP_VERSION` 环境变量覆盖 manifest 的 id/version。

## Common Pitfalls
- `scripts/package.ts` 对 `manifest.assets` 路径做白名单正则校验（`^[a-zA-Z0-9_.\-/]{1,160}$`、拒绝 `.`/`..` 段）——新增素材路径不合法会直接抛错
- ZIP 只收固定基础文件（index.html、app.js、app.css、strings.json 等）+ manifest 声明的 assets；漏声明 = 不入包
- `minify: false` 是有意为之（审核可读性），不要"优化"掉
- 沙箱构建 7 天过期、同 SHA-256 重复上传会被拒（BUILD_EXISTS）

## Related
- [[quality-rules]] / [[coding-conventions]] — convention specs
