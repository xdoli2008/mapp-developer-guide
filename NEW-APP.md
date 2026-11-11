# 从本模板创建新微应用

本仓库已设为 GitHub 模板仓库（Template Repository），当前状态（docs + sdk + demos + .workflow 工作流）即模板基线。所有新微应用项目从这里起步。

## 1. 创建应用仓库

GitHub 仓库页 → 绿色「Use this template」按钮 → 命名新仓库；或命令行：

```bash
gh repo create my-app --template xdoli2008/mapp-developer-guide --clone
```

## 2. 创建应用目录

**保留 `sdk/` 原样不动**——SDK 未发布 npm，应用经 `file:../../sdk` 引用本地 SDK，目录相对层级（`<app>/` → 仓库根 → `sdk/`）不能变。

复制最小示例起步（与 `demos/` 同级，深度一致）：

```bash
cp -r demos/brick-breaker apps/my-app
```

需要更全的能力参考时再看 `demos/game-2048`（基础四权限）与 `demos/capability-lab`（18 项权限全覆盖）。

## 3. 替换身份信息（三处）

| 位置 | 改什么 |
|------|--------|
| `apps/my-app/package.json` | `name` 改为自己的包名（如 `@discourse-mini-app/app-my-app`） |
| `apps/my-app/manifest.json` | `id`：先在社区个人资料「应用页」创建应用，**原样复制平台分配的 ID，不要自行生成**；`name`/`version` 按应用实际填写 |
| `apps/my-app/src/` | 清空示例逻辑，保留 `main.ts`/`host.ts`/`template.html`/`style.css` 骨架与 `vite.config.ts`/`tsconfig.json`/`scripts/package.ts` |

不需要的 `demos/` 可删除（保留作参考亦可）。

## 4. 安装、开发、构建

```bash
cd apps/my-app
bun install
bun run dev          # 本地开发（host stub 模式，端口见 vite.config.ts）
bun run build        # 产物 dist/package/app.zip
bun run build:debug  # 带 source map 的沙箱调试包（7 天有效）
bun test test        # 单元测试
```

上传 `app.zip` 到社区应用管理页走沙箱检查，提审发布见 `docs/submission-review.md`。

## 5. 重置 .workflow 工作流（新仓库必做）

模板携带的 `.workflow/` 是本仓库（文档维护向）的状态，新应用仓库要重置：

- `.workflow/project.md` — 重写 What/Core Value/Requirements 为新应用的目标（可参考结构保留）
- `.workflow/state.json` — `project_name` 改成新仓库名，`milestones`/`artifacts` 保持空，status 保持 `idle`
- 重建代码索引 — kg 数据库不入库，新仓库需运行：`maestro kg index`
- **可直接沿用**：`specs/`（构建/测试/编码约定）、`knowhow/`（test/build/dev/debug 四个配方）、`domain/glossary.yaml`（13 个平台术语）——对所有微应用通用

## 约束速记

- manifest `id` 由社区分配，原样复制；改素材需重新打包提审
- `app.zip` 内不放访问令牌或其他秘密
- 限额：解压 128 MiB / 单文件 32 MiB / 1024 个文件；大素材走 manifest 声明的 CDN（SHA-256 钉死）
- 沙箱构建 7 天过期；同 SHA-256 重复上传被拒（`BUILD_EXISTS`）
