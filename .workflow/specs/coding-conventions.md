---
title: "Coding Conventions"
readMode: required
priority: high
category: coding
keywords:
  - style
  - naming
  - import
  - pattern
  - convention
  - formatting
---

# Coding Conventions

Auto-generated from project analysis (2025-11-11 init). Update manually as patterns evolve.

## Formatting

- Indentation: 2 spaces（无 .editorconfig/.prettierrc，以现有代码为准）
- Quotes: double quotes
- Semicolons: yes
- Trailing commas: yes（多行结构）

## Naming

- Variables/functions: camelCase（`createGame`、`movePaddle`、`launchBall`）
- Types/interfaces: PascalCase（`TemplateElements`、`Game`、`MiniAppManifest`）
- Constants: UPPER_SNAKE_CASE（`WIDTH`、`HEIGHT`）
- Files: kebab-case 为主（`pointer-lock.ts`、`web-audio.ts`），核心短文件名（`main.ts`、`host.ts`、`physics.ts`）
- Demo npm scope: `@discourse-mini-app/demo-*`

## Imports

- Style: named imports；类型一律 `import type { ... }`（`verbatimModuleSyntax: true` 强制）
- 相对导入带显式 `.ts` 扩展名（`from "./physics.ts"`，Bun 风格，`allowImportingTsExtensions`）
- Path aliases: none
- JSON 以默认导入使用（`import messages from "./strings.json"`，`resolveJsonModule`）
- 本地 SDK 通过 `file:../../sdk` 引用，导入 `@discourse-mini-app/sdk` 及子路径（`/pack`、`/server` 等）

## Patterns

- 无 UI 框架：原生 DOM + `addEventListener` + `textContent`；模板元素经 `document.getElementById` + 类型映射（`TemplateElements`）取用，不用 innerHTML 插值
- 入口结构：`main.ts`（组装）→ `host.ts`（SDK 宿主桥接，dev 模式传 `null` 走本地 stub）→ 领域模块（如 `physics.ts` 纯逻辑、可测试）
- Vite 插件 `demo-body` 在构建时把 `src/template.html` 注入 `index.html` 的 `<!--app-->` 占位符——index.html 是正文片段，禁内联脚本/样式
- 构建产物固定为单 IIFE `app.js` + `app.css`（`cssCodeSplit: false`、`minify: false`）
- 打包脚本 `scripts/package.ts` 统一模式：读 `manifest.json` → 收集 dist/web 固定文件 → `pack()` 生成 app.zip，并对 manifest.assets 路径做白名单正则校验
- 测试只测纯逻辑模块（physics/engine/state），DOM/SDK 桥接层（host.ts）通过 stub 测试连接状态

## Entries
