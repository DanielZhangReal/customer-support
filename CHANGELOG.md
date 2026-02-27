# Changelog

版本号遵循 [Semantic Versioning](https://semver.org/)：
- **patch**（x.x.X）：`knowledge/` 内容更新
- **minor**（x.X.x）：SKILL.md 行为逻辑、intent 路由变更
- **major**（X.x.x）：架构重构、权限模型变更、SOUL.md 更新

---

## [2.1.0] - 2026-02-27

### 新增
- `config/admins.yaml`：新增 GitHub API 集成，`GITHUB_TOKEN`、`GITHUB_REPO`、`GITHUB_BASE_BRANCH` 环境变量声明
- 知识库回滚流程说明

### 变更
- `config/admins.yaml`：`superadmin`（单对象）改为 `superadmins`（列表），支持多个 superadmin
- `SKILL.md`：admin 知识库更新方式从手动 git PR 改为 Bot 调用 GitHub API 直接提交
- `SKILL.md`：admin 管理命令说明更新，明确 Bot 只生成配置预览，superadmin 手动提交
- `SKILL.md`：版本管理规则明确区分 knowledgeVersion（admin 更新）与 version（superadmin 更新）
- `SOUL.md`：admin/superadmin 权限描述修正，移除与安全边界矛盾的"执行系统命令"表述
- 统一大小写规范：角色名全部小写（`superadmin`/`admin`/`user`），YAML key 使用复数（`superadmins`/`admins`）

### 修复
- 移除 `SOUL.md` 中对已删除文件的引用（`product-info.md`、`glossary.md`、`AGENTS.md`）
- 修正 `GITHUB_TOKEN` 所需权限声明（只需 `contents:write`，移除多余的 `pull-requests:write`）
- 修正添加管理员命令格式说明，备注名不允许含空格

---

## [2.0.0] - 2026-02-27

### 重大变更（Breaking Changes）
- 权限配置文件从根目录 `access-control.md` 迁移至 `config/access-control.md`
- admin 知识库更新方式从对话内直接修改改为 git PR 工作流
- 平台支持扩展：新增 Discord（原仅支持 Telegram）

### 新增
- `config/access-control.md`：支持 Telegram + Discord 双平台 ID 配置
- `knowledge/_index.md`：标签索引，包含 knowledgeVersion 追踪
- `knowledge/overview.md`：产品/项目概述模板
- `knowledge/guides.md`：使用指南模板
- `knowledge/community.md`：社区渠道与联系方式模板
- `CHANGELOG.md`：本文件

### 变更
- `SKILL.md`：完整重写，新增 Discord 支持、git PR 工作流规范、版本号管理
- `SOUL.md`：更新配置文件路径引用，新增 Discord 身份识别规则
- `README.md`：更新文件结构说明，新增 git PR 工作流文档
- `CLAUDE.md`：更新架构说明

### 移除
- 根目录 `access-control.md`（已迁移至 `config/`）
- `knowledge/glossary.md`（内容合并至 `knowledge/overview.md`）
- `knowledge/product-info.md`（内容合并至 `knowledge/overview.md`）

---

## [1.0.0] - 2026-02-26

### 初始版本
- 基于 Telegram UID 的三级权限系统（superadmin / admin / user）
- 6 个知识库文件（faq、product-info、policies、troubleshooting、pricing、glossary）
- 对话内直接知识库更新命令
