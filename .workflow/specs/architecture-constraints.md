---
title: "Architecture Constraints"
readMode: required
priority: high
category: arch
keywords:
  - architecture
  - module
  - boundary
  - dependency
  - constraint
---

# Architecture Constraints

Auto-generated from project structure (2025-11-11 init). Update manually as architecture evolves.

## Module Structure

- Type: multi-package 仓库（docs + sdk + demos/，无 workspace 根 package.json）
- Key modules:
  - `docs/` — 13 篇中文开发文档（Markdown），平台开发者的主要交付物
  - `sdk/` — `@discourse-mini-app/sdk` v0.3.0 发布物形态：`.mjs` 压缩产物 + `.d.ts` 类型声明，**非可重构源码**，按平台发布物对待
  - `demos/brick-breaker` — 最小可用示例（user/theme/localStorage）
  - `demos/game-2048` — 基础四权限组合示例
  - `demos/capability-lab` — 18 项权限全覆盖能力测试台
  - `.orca/` — 空，占位

## Layer Boundaries

- 单个 demo 内部：`main.ts`（入口组装）→ `host.ts`（SDK 宿主桥接/身份渲染）→ 领域纯逻辑模块（`physics.ts`/`engine.ts`/`state.ts` 等）
- `scripts/package.ts` 是构建期脚本（Node/Bun fs），不属于运行时代码
- SDK 是唯一运行时依赖边界：demo 不直接触碰平台宿主 API，一切经 `@discourse-mini-app/sdk`

## Dependency Rules

- demos → `@discourse-mini-app/sdk`（`file:../../sdk` 本地引用）
- demo 之间零依赖、零共享代码（各自完整自持）
- sdk 不依赖 demos；docs 中的示例代码引用 SDK API（以 `sdk/*.d.ts` 为契约）

## Technology Constraints

- Runtime: Bun >= 1.4.3（capability-lab 要求 >=1.4.3，另两个 >=1.2.0）
- Build: Vite 8.3.5（单 IIFE lib 模式，target es2022，minify: false）
- TypeScript 5.9.3，`strict: true`、`verbatimModuleSyntax: true`、`module: Preserve` / `moduleResolution: Bundler`、`noEmit: true`
- Module system: ESM（`"type": "module"`）
- 每个包私有（`private: true`），无发布流程

## Entries
