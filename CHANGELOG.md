# Changelog

版本号遵循 [Semantic Versioning](https://semver.org/)：
- **patch**（x.x.X）：`knowledge/` 内容更新
- **minor**（x.X.x）：SKILL.md 行为逻辑、intent 路由变更
- **major**（X.x.x）：架构重构、权限模型变更、SOUL.md 更新

---

## [2.0.0] - 2026-02-27

### 重大变更（Breaking Changes）
- 权限配置文件从根目录 `access-control.md` 迁移至 `config/access-control.md`
- Admin 知识库更新方式从对话内直接修改改为 git PR 工作流
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
