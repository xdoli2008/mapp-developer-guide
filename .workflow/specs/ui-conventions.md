---
title: "UI Conventions"
readMode: required
priority: normal
category: ui
keywords:
  - ui
  - dom
  - css
  - template
---

# UI Conventions

Auto-generated from project analysis (2025-11-11 init). 无 UI 框架。

## Patterns

- 原生 DOM：`document.getElementById` + 类型映射取元素（`TemplateElements` 模式），`addEventListener` 绑事件，`textContent` 更新文案——不用 innerHTML 拼接
- `index.html` 是正文片段：由 Vite `demo-body` 插件把 `src/template.html` 注入 `<!--app-->` 占位符生成；禁内联脚本/事件属性
- 样式集中于单一 `app.css`（构建 `cssCodeSplit: false`）；`src/style.css` 为源
- 文案外置 `strings.json` 默认导入（i18n 预留模式）
- Canvas 用于游戏渲染（`<canvas id="court">` 模式）

## Entries
