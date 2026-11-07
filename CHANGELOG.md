# 变更记录

本文件记录星云操作系统（NebulaOS）的所有重要变更，格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [SemVer 2.0.0](https://semver.org/lang/zh-CN/)。

## [2.0.0] - 2026-11-07

### 新增

- 补齐开源社区健康文件：`LICENSE`、`CODE_OF_CONDUCT.md`、`SECURITY.md`、`CHANGELOG.md`
- 新增 `docs/README.md` 全局文档索引，串联标规文档、开发指南与验收体系
- 新增 ESLint 9 Flat Config（`eslint.config.mjs`），接入 `eslint-config-next` 16
- 新增 `pnpm-workspace.yaml`，按 pnpm 12 规范通过 `allowBuilds` 管理构建脚本，并设置 `minimumReleaseAge: 60` 供应链策略
- `package.json` 增加仓库元数据：`description`、`keywords`、`repository`、`bugs`、`homepage`、`author`、`license`、`engines`
- 新增 `verify` 脚本作为本地与 CI 的统一质量门禁
- 项目英文名定为 **NebulaOS**，仓库迁移至 `YYC3-Nebula-OS`，同步更新徽章、克隆地址与包元数据

### 变更

- **升级 Next.js 14.2.25 → 16.4.5**（Turbopack 默认、App Router 增强）
- **升级 React / React DOM 19 → 19.3.1**，`@types/react` / `@types/react-dom` 同步至 19.3
- **升级 Tailwind CSS 3.4 → 4.3**：改用 `@tailwindcss/postcss`，主题迁移至 `@theme`，移除 `tailwind.config.js`
- 动画插件由 `tailwindcss-animate` 迁移至 `tw-animate-css`
- TypeScript 固定至 5.9.3；新增 `tsx` 支撑权限校验脚本
- CI 工作流升级至 Node 22 + `pnpm/action-setup@v4`，使用冻结锁文件
- README 全面重写：品牌顶图、徽章矩阵、技术栈、架构与可视化交互体系、标签关键词、文档导航
- 开发服务器端口调整为 3169
- 质量门禁脚本由 `ci` 更名为 `verify`，避免与 pnpm 内置 `pnpm ci` 命令冲突

### 移除

- 移除 `autoprefixer`（Tailwind CSS 4 内置处理）
- 移除 `tailwindcss-animate` 依赖与根级 `tailwind.config.js`

### 修复

- 修复 `pnpm install` 在 pnpm 12 下的冻结锁文件失败问题
- 修复缺失的 `tsx` 依赖导致 `validate:permissions` 无法执行
- 修复项目缺少 ESLint 配置导致 `pnpm lint` 不可用的问题
- 修复权限字符串不一致：`view:ai-insights` → `view:insights`，权限矩阵改用 `PERMISSIONS` / `PermissionLabels` 单一数据源
- **清零 30 处存量类型错误**：自定义图标类型、framer-motion 变体、`RadialProgressChart` 必填属性、行业适配字段与页面类型等

## [1.0.0] - 2025-01-25

### 新增

- AI 智能分析引擎：移动平均预测、Z-score 异常检测、线性回归趋势与建议生成
- 多租户 RBAC：5 级角色、12 项权限、权限门控组件
- 5 类高级图表：折线图、热力图、径向进度、区域对比、实时仪表盘
- 实时通知中心、活动流与团队在线状态
- 全平台响应式与完整中文本地化
- 健康检查接口 `/api/health`
