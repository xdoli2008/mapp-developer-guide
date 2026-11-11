---
title: "mapp-developer-guide — Test Workflow"
type: recipe
tags: [workflow, test-workflow, bun-test, auto-generated]
created: 2025-11-11T16:30:00+08:00
source: spec-setup
---

# mapp-developer-guide — Test Workflow

## Goal
在任一 demo 包内运行 bun:test 单元测试并验证通过。

## Prerequisites
- Bun >= 1.4.3（`bun --version` 确认）
- 无需安装依赖以外的环境；demo 通过 `file:../../sdk` 解析本地 SDK（如未装过依赖先 `bun install`）

## Steps
1. 进入目标 demo 目录：`cd demos/brick-breaker`（或 game-2048 / capability-lab）
2. 运行全量测试：`bun test test`
3. 过滤单个文件：`bun test test/physics.test.ts`
4. 过滤单个用例名：`bun test -t "rebounds"`

## Expected Outcome
所有 `*.test.ts` 通过，终端显示每个文件的 pass 数与总耗时，退出码 0。

## Common Pitfalls
- 必须在 demo 目录内运行——仓库无根级聚合测试命令，从仓库根直接 `bun test` 会扫不到或扫错范围
- capability-lab 要求 Bun >= 1.4.3，旧版 Bun 可能缺 API
- 测试不依赖浏览器环境；如遇 DOM 相关失败，说明被测模块不应触碰 document（host 桥接需传 stub）

## Related
- [[test-conventions]] / [[architecture-constraints]] — convention specs
