# OpenClaw Customer Support Skill

通用智能客服 Skill 模板，支持 Telegram 和 Discord 双平台，基于知识库自动回答问题，三级权限管理，知识库通过 git PR 维护。

## 文件结构

```
customer-support/
├── SOUL.md                      ← AI 人格定义、安全规则（SuperAdmin 才能修改）
├── SKILL.md                     ← Skill 行为定义：意图路由、回复规范、权限逻辑
├── CHANGELOG.md                 ← 版本变更记录
├── README.md                    ← 本文件
├── config/
│   └── access-control.md        ← 权限配置：SuperAdmin 和 Admin 的平台 ID（SuperAdmin 才能修改）
└── knowledge/                   ← 知识库（Admin 可通过 git PR 修改）
    ├── _index.md                ← 知识库版本 + 标签索引（每次 PR 必须更新）
    ├── overview.md              ← 产品/项目概述、核心功能、术语
    ├── faq.md                   ← 常见问题解答
    ├── guides.md                ← 使用指南、快速上手
    ├── pricing.md               ← 定价方案（可选，不适用则删除）
    ├── policies.md              ← 退款、隐私、服务条款
    ├── troubleshooting.md       ← 故障排查、错误码
    └── community.md             ← Discord/Telegram 社区、联系方式
```

---

## 安装

### 方式一：ClawHub
```bash
clawhub install customer-support
```

### 方式二：手动安装
```bash
git clone <repo-url>
mv customer-support ~/.openclaw/workspace/skills/customer-support
```

安装完成后在对话中说「刷新技能」生效。

---

## 初始配置（必做）

### 1. 配置权限

编辑 `config/admins.yaml`，填入 SuperAdmin 和 Admin 的平台 ID：

```yaml
superadmins:
  - name: "Owner"
    telegram: "你的 Telegram UID"   # 向 @userinfobot 发消息获取
    discord: "你的 Discord 用户 ID" # Discord 开发者模式 → 右键用户 → 复制 ID

admins:
  - name: "管理员 1"
    telegram: "UID"
    discord: "用户 ID"
    added_by: "telegram:superadmin_uid"
    added_at: "YYYY-MM-DD"
```

### 2. 填写知识库

用实际业务内容替换 `knowledge/` 下各文件中的 `[...]` 占位符，从 `overview.md` 开始。

替换前先全局搜索占位符：
```bash
grep -r "\[" knowledge/
grep -r "YYYY-MM-DD" knowledge/
grep -r "example.com" knowledge/
```

### 3. 刷新技能

在 OpenClaw 对话中说「刷新技能」应用所有更改。

---

## 知识库维护

### Admin 更新知识库（仅限 `knowledge/`）

1. 在对话中告诉 Bot 要更新什么，Bot 会整理好格式给你确认
2. Admin 确认后，Bot 自动调用 GitHub API 提交到 main 分支
3. 说「刷新技能」生效

> 需在环境变量中配置 `GITHUB_TOKEN`（`contents:write` 权限）、`GITHUB_REPO`（`owner/repo`）、`GITHUB_BASE_BRANCH`（默认 `main`）。

### SuperAdmin 更新系统文件

系统文件（SOUL.md、SKILL.md、config/ 等）只能通过手动 git PR 修改，提交时需同步更新 `SKILL.md` 版本号和 `CHANGELOG.md`。

### 版本号规则

| 变更类型 | 版本号 | 示例 |
|---------|--------|------|
| `knowledge/` 内容更新 | patch（x.x.**X**） | 1.0.0 → 1.0.1 |
| SKILL.md 逻辑/路由变更 | minor（x.**X**.x） | 1.0.0 → 1.1.0 |
| 架构重构、SOUL.md、权限模型变更 | major（**X**.x.x） | 1.0.0 → 2.0.0 |

---

## 权限说明

| 角色 | 能做什么 | 不能做什么 |
|------|---------|----------|
| **SuperAdmin** | 全部权限 | — |
| **Admin** | 通过 PR 更新 `knowledge/`、知识库查询 | 修改系统文件、管理权限 |
| **User** | 知识库查询 | 所有写操作 |

身份识别基于平台用户 ID（Telegram UID / Discord 用户 ID），与用户名无关。

---

## 平台支持

| 平台 | 身份字段 | 获取方式 |
|------|---------|---------|
| Telegram | 数字 UID（`from.id`） | 向 [@userinfobot](https://t.me/userinfobot) 发消息 |
| Discord | Snowflake 用户 ID（`user.id`） | 开发者模式 → 右键用户 → 复制用户 ID |

---

## License

MIT
