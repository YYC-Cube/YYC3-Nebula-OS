# 贡献指南

> **YYC³**（YanYuCloudCube™ · 注册商标）<br>
> 言启象限 | 语枢未来

感谢您对 **星云操作系统（NebulaOS）** 的关注。本指南说明如何搭建环境、提交变更并通过质量门禁，帮助您的贡献顺利合入。

## 目录

- [行为准则](#行为准则)
- [环境准备](#环境准备)
- [开发工作流](#开发工作流)
- [提交规范](#提交规范)
- [编码规范](#编码规范)
- [质量门禁](#质量门禁)
- [Pull Request 检查清单](#pull-request-检查清单)
- [评审与合入](#评审与合入)
- [文档贡献](#文档贡献)
- [安全问题](#安全问题)
- [许可证](#许可证)

## 行为准则

参与本项目即表示您同意遵守 [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md)。请保持尊重、专业与建设性。

## 环境准备

| 依赖 | 版本要求 |
| ---- | ---- |
| Node.js | ≥ 20.9（推荐 22 LTS） |
| pnpm | ≥ 9（推荐 12） |
| Git | 最新稳定版 |

```bash
# 1. Fork 并克隆
git clone https://github.com/<your-name>/YYC3-Nebula-OS.git
cd YYC3-Nebula-OS

# 2. 安装依赖
pnpm install

# 3. 启动开发服务器（默认 3169）
pnpm dev
```

访问 <http://localhost:3169> 验证环境。

## 开发工作流

1. 从 `develop` 拉取最新代码并创建分支；
2. 完成变更并本地自测；
3. 运行质量门禁；
4. 提交 PR 至 `develop`（紧急修复可直接指向 `main`，需说明原因）。

**分支命名**：

| 前缀 | 用途 | 示例 |
| ---- | ---- | ---- |
| `feature/` | 新功能 | `feature/ai-anomaly-panel` |
| `fix/` | 缺陷修复 | `fix/rbac-gate-bypass` |
| `docs/` | 文档更新 | `docs/readme-badges` |
| `refactor/` | 重构 | `refactor/chart-render` |
| `perf/` | 性能优化 | `perf/lazy-charts` |
| `test/` | 测试相关 | `test/auth-permissions` |
| `chore/` | 构建与工具 | `chore/upgrade-next16` |

## 提交规范

遵循 [Conventional Commits](https://www.conventionalcommits.org/zh-hans/)，提交信息使用中文描述：

```text
<type>(<scope>): <subject>

<body>

<footer>
```

| type | 说明 |
| ---- | ---- |
| `feat` | 新功能 |
| `fix` | 缺陷修复 |
| `docs` | 文档变更 |
| `style` | 格式调整（不影响逻辑） |
| `refactor` | 重构（非功能、非修复） |
| `perf` | 性能优化 |
| `test` | 测试相关 |
| `build` / `chore` | 构建、依赖、工具链 |
| `ci` | CI 配置 |

示例：

```bash
git commit -m "feat(ai): 新增异常检测阈值配置"
git commit -m "fix(auth): 修复 viewer 越权访问设置页"
git commit -m "docs(readme): 同步 Next.js 16 技术栈与徽章"
```

## 编码规范

### 通用

- 遵循 [YYC³ 团队通用-命名规范](./docs/YYC3-团队通用-标规文档/YYC3-团队通用-命名规范.md)；
- 新增代码文件需包含 YYC³ 标准 JSDoc 标头（`file` / `description` / `author` / `version` / `created` / `updated` / `status` / `tags`）；
- 品牌正文写作 `YYC³`，路径与代码标识使用 ASCII `YYC3`；
- 禁止提交密钥、令牌、真实个人信息与 `.env*` 文件。

### TypeScript / React

- 使用函数组件与 Hooks，组件名 PascalCase；
- Props 必须显式声明类型，避免 `any`；
- 明确 Server / Client Component 边界，仅在需要交互或浏览器 API 时添加 `"use client"`；
- 副作用放入 `useEffect`，派生数据优先 `useMemo`，避免渲染期调用 `Date.now()`、`Math.random()` 等非纯函数（新代码强制）；
- 复用 `hooks/` 与 `lib/` 中的既有能力，避免重复实现。

### 样式与组件

- 使用 Tailwind CSS 4 工具类，禁止内联样式；
- 设计令牌统一维护在 [`app/globals.css`](./app/globals.css) 的 `@theme` 中，**不要再新增 `tailwind.config.js`**；
- 语义色使用 `bg-background`、`text-muted-foreground` 等令牌类，禁止硬编码十六进制颜色；
- 新组件优先基于 [`components/ui`](./components/ui) 组合，保持 Radix 无障碍语义；
- 移动端优先，断点遵循项目既有约定（768px）。

### 权限

- 新增权限点须同步更新 `lib/auth/types.ts` 与 `lib/auth/permissions.ts`；
- 受保护 UI 使用 `PermissionGate`，路由级校验放在 `AuthGuard`；
- 变更后运行 `pnpm validate:permissions`。

## 质量门禁

提交 PR 前，以下命令必须全部通过：

```bash
pnpm lint                  # ESLint 9 Flat Config
pnpm typecheck             # TypeScript 类型检查
pnpm validate:permissions  # 权限映射合法性
pnpm build                 # Next.js 16 生产构建
# 或一次性执行
pnpm verify
```

> 存量代码中的 `react-hooks/purity` 等规则暂列为 warning，新代码不应新增告警。

## Pull Request 检查清单

- [ ] 变更范围聚焦，未混入无关格式化
- [ ] 本地 `pnpm verify` 通过
- [ ] 新增/修改逻辑具备类型定义
- [ ] 涉及 UI 的变更已自测桌面端与移动端
- [ ] 涉及权限的变更已同步权限常量与校验
- [ ] 更新了相关文档（README / docs / JSDoc）
- [ ] 提交信息符合 Conventional Commits
- [ ] 无密钥、凭据或敏感数据

## 评审与合入

1. 维护者将在 3 个工作日内完成首轮评审；
2. 至少 1 名维护者批准；
3. CI 全绿后方可合入；
4. 合并策略：Squash Merge，保留清晰的提交语义；
5. 破坏性变更需在描述中标注 `BREAKING CHANGE` 并同步 CHANGELOG。

## 文档贡献

- 面向团队的规范文档放入 `docs/`，遵循 [文档闭环标准](./docs/YYC3-团队通用-标规文档/YYC3-团队通用-文档闭环.md)；
- 标准文档必须包含 YAML Front Matter 与文末品牌页脚；
- 目录与文件命名遵循 `YYC3-{主题}.md` 或 `{序号}-YYC3-{功能}-{描述}.md`；
- 更新文档时同步 `updated`、`version` 与「变更历史」。

Front Matter 模板：

```yaml
---
file: YYC3-示例文档.md
title: YYC³ 示例文档 — 主题
description: 一句话说明文档内容，不超过 50 字
author: YYC³ Team <admin@0379.email>
version: v1.0.0
created: 2026-11-07
updated: 2026-11-07
status: stable
tags: [guide, document, standard]
category: guide
language: zh-CN
audience: developers
complexity: basic
---
```

## 安全问题

请勿通过公开 Issue 报告安全漏洞，流程见 [SECURITY.md](./SECURITY.md)。

## 许可证

提交贡献即表示您同意将代码以本项目的 [MIT License](./LICENSE) 发布。

---

再次感谢您的贡献。**言启千行代码，语枢万物智能。**

---

<p align="center">
  <b>YYC³</b>（YanYuCloudCube™ · 注册商标）<br>
  言启象限 | 语枢未来<br>
  <i>Words Initiate Quadrants, Language Serves as Core for Future</i><br>
  万象归元于云枢 | 深栈智启新纪元<br>
  <i>All things converge in cloud pivot; Deep stacks ignite a new era of intelligence</i><br>
  <sub>© 2026 YanYuCloudCube™（YYC³）· <a href="mailto:admin@0379.email">admin@0379.email</a></sub>
</p>
