---
title: "mapp-developer-guide — Debug Workflow"
type: recipe
tags: [workflow, debug-workflow, mini-app-debug, auto-generated]
created: 2025-11-11T16:30:00+08:00
source: spec-setup
---

# mapp-developer-guide — Debug Workflow

## Goal
产出并使用带 source map 的沙箱调试包，在平台沙箱中定位线上问题。

## Prerequisites
- Bun >= 1.4.3；能访问 Discourse 社区的微应用沙箱上传入口
- 调试包有效期：沙箱构建 7 天过期

## Steps
1. 进入目标 demo 目录：`cd demos/brick-breaker`
2. 构建调试包：`bun run build:debug`（`MINI_APP_DEBUG=1` 使 `vite.config.ts` 中 `sourcemap: "inline"` 生效）
3. 上传 `dist/package/app.zip` 到社区沙箱（个人资料页 → 创建/更新应用 → 沙箱构建）
4. 在沙箱 iframe 中用浏览器 DevTools 定位；inline source map 将压缩/打包后的 `app.js` 映射回 `src/*.ts`
5. 本地纯逻辑问题优先用 `bun test test` 复现（不起浏览器）

## Expected Outcome
沙箱内报错栈可映射到 TypeScript 源文件行号；问题可在本地复现修复后重新 `build:debug` 验证。

## Common Pitfalls
- 普通产物（`bun run build`）无 source map，线上栈不可读——调试必须用 `build:debug`
- 沙箱构建 7 天过期、同 SHA-256 重复上传被拒（BUILD_EXISTS）：重新上传前确保内容有变化
- DEV 模式（`bun run dev`）的 host stub 与真实宿主行为有差异，宿主桥接类问题（身份、主题、权限）必须在沙箱验证

## Related
- [[build-workflow]] / [[test-workflow]] — 配套流程
