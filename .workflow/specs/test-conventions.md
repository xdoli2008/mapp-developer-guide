---
title: "Test Conventions"
readMode: required
priority: normal
category: test
keywords:
  - test
  - bun test
  - coverage
  - convention
---

# Test Conventions

Auto-generated from project analysis (2025-11-11 init). Update manually as patterns evolve.

## Framework

- Framework: bun:test（`import { describe, expect, test } from "bun:test"`）
- Run command: `bun test test`（在各自 demo 目录内运行；仓库无根级统一测试命令）

## Directory Structure

- Pattern: 独立 `test/` 目录（与 `src/` 平级，不 co-locate）

## Naming Conventions

- Test files: `*.test.ts`（`host.test.ts`、`physics.test.ts`）

## Patterns

- 只测纯逻辑模块（physics/engine/state/pointer/pointer-lock/web-audio 的可测部分）；DOM/SDK 桥接经 host stub 注入测试
- `describe` 按模块主题命名（如 `"breakout physics"`），`test` 描述行为句式
- 断言用 `expect` + `toBe`/`toBeLessThan`/`toBeGreaterThan` 等；无 mock 框架依赖
- 三个 demo 均有测试；docs 与 sdk 发布物不承载测试

## Entries
