# 安全政策

> **YYC³**（YanYuCloudCube™ · 注册商标）<br>
> 言启象限 | 语枢未来

感谢您帮助保护星云操作系统（NebulaOS）及其用户。我们高度重视安全漏洞的负责任披露。

## 支持版本

| 版本 | 支持状态 |
| ---- | ---- |
| 2.x（Next.js 16 / React 19） | ✅ 积极维护 |
| 1.x（Next.js 14） | ⚠️ 仅关键安全修复 |
| < 1.0 | ❌ 不再支持 |

## 报告漏洞

**请勿通过公开 Issue、讨论区或 PR 披露安全漏洞。**

请发送邮件至 **admin@0379.email**，邮件主题以 `[SECURITY]` 开头，并尽量包含：

- 漏洞类型与影响范围
- 受影响的版本、路由或组件
- 复现步骤或最小复现仓库
- 概念验证（PoC）与可能的攻击路径
- 您希望公开披露的时间线（如有）
- 联系方式（便于后续沟通）

如涉及敏感数据，请使用加密附件或先邮件告知，我们将提供安全的上传方式。

## 响应时限

| 阶段 | 目标 |
| ---- | ---- |
| 确认收到 | 48 小时内 |
| 初步评估与定级 | 5 个工作日内 |
| 修复计划与时间线 | 评估后 5 个工作日内 |
| 修复发布 | 按严重程度协商，Critical 优先 |
| 公开披露 | 修复发布后协调进行，默认给予报告者致谢 |

## 安全范围

**在范围内**

- 认证、授权与 RBAC 绕过（`lib/auth`、`components/auth`）
- 越权访问受保护路由或数据
- 跨站脚本（XSS）、注入、原型污染
- 敏感信息泄露（密钥、令牌、个人数据）
- 依赖供应链风险（恶意包、已知高危 CVE）
- 安全响应头缺失或错误配置

**不在范围内**

- 需要物理接触或已取得管理员权限的攻击
- 社会工程、钓鱼与垃圾邮件
- 仅影响不受支持版本的漏洞
- 自动化扫描产生的低价值报告（无实际影响证明）
- 理论上存在但无法复现的问题

## 安全基线

本项目已采取以下措施：

- 生产依赖与构建脚本白名单（`pnpm.onlyBuiltDependencies`）
- 响应头：`X-Content-Type-Options: nosniff`、`X-Frame-Options: DENY`、`Referrer-Policy: strict-origin-when-cross-origin`
- 服务端渲染与客户端边界隔离，敏感逻辑不下发浏览器
- 权限校验集中在 `lib/auth`，UI 层通过 `PermissionGate` 二次门控
- 锁文件固定依赖版本，CI 使用 `--frozen-lockfile`

## 安全建议（部署方）

- 通过环境变量注入密钥，禁止提交 `.env*` 到仓库
- 在生产环境启用 HTTPS 与 HSTS
- 按需补充 CSP（`Content-Security-Policy`）与 `Permissions-Policy`
- 定期执行 `pnpm audit` 与依赖升级
- 将 `admin@0379.email` 替换为您所在组织的安全响应邮箱（如适用）

## 披露政策

我们遵循协调披露原则：

1. 报告者私下提交漏洞；
2. 维护者确认、评估并修复；
3. 修复发布后，与报告者协商公开时间与内容；
4. 经报告者同意，在变更记录与安全公告中致谢。

请勿在修复发布前公开漏洞细节。

## 致谢

感谢所有以负责任方式帮助提升本项目安全性的研究者与贡献者。

---

<p align="center">
  <b>YYC³</b>（YanYuCloudCube™ · 注册商标）<br>
  言启象限 | 语枢未来<br>
  <i>Words Initiate Quadrants, Language Serves as Core for Future</i><br>
  万象归元于云枢 | 深栈智启新纪元<br>
  <sub>© 2026 YanYuCloudCube™（YYC³）· <a href="mailto:admin@0379.email">admin@0379.email</a></sub>
</p>
