---
title: "Quality Rules"
readMode: required
priority: normal
category: quality
keywords:
  - quality
  - typecheck
  - gate
  - verification
---

# Quality Rules

Auto-generated from project analysis (2025-11-11 init). 无 eslint/prettier/biome 配置；质量门禁为 typecheck + 测试。

## Gates

- `typecheck`: 每包 `tsc -p tsconfig.json`（strict 模式），且内置于 `build` 脚本链首位——typecheck 不过则不打包
- `test`: 每包 `bun test test`
- `build`: `typecheck → vite build → scripts/package.ts`，产物为 `dist/web/` + `dist/package/app.zip`
- SDK `.d.ts` 是文档与 demo 共同的类型契约源；修改 SDK 相关描述必须回读对应 `.d.ts`

## Entries
