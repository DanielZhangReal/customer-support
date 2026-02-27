# OpenClaw Customer Support Skill

通用智能客服 Skill 模板，支持 Telegram 和 Discord 双平台，基于知识库自动回答问题，三级权限管理，知识库通过 GitHub API 自动提交维护。

## 文件结构

```
customer-support/
├── SOUL.md                      ← AI 人格定义、安全规则（superadmin 才能修改）
├── SKILL.md                     ← Skill 行为定义：意图路由、回复规范、权限逻辑
├── CHANGELOG.md                 ← 版本变更记录
├── README.md                    ← 本文件
├── config/
│   ├── admins.yaml              ← 权限数据：superadmins 和 admins 的平台 ID（superadmin 专属；add/remove admin 可通过对话自动提交，superadmins 列表须手动编辑）
│   └── access-control.md        ← 权限文档：角色说明与权限矩阵
└── knowledge/                   ← 知识库（admin 通过对话更新，Bot 自动提交）
    ├── _index.md                ← 知识库版本 + 标签索引（每次 Bot 提交时自动更新）
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

编辑 `config/admins.yaml`，填入 superadmin 和 admin 的平台 ID：

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

### admin 更新知识库（仅限 `knowledge/`）

1. 在对话中告诉 Bot 要更新什么，Bot 会整理好格式给你确认
2. admin 确认后，Bot 自动调用 GitHub API 提交到 $GITHUB_BASE_BRANCH（默认 main）
3. 说「刷新技能」生效

> 需在环境变量中配置 `GITHUB_TOKEN`（`contents:write` 权限）、`GITHUB_REPO`（`owner/repo`）、`GITHUB_BASE_BRANCH`（默认 `main`）。

### superadmin 更新系统文件

- **SKILL.md / SOUL.md**：只能通过手动 git PR 修改，提交时需同步更新版本号和 `CHANGELOG.md`
- **其他 config/ 文件**：可通过对话由 Bot 自动提交，流程与知识库更新相同

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
| **superadmin** | 全部权限 | — |
| **admin** | 通过对话更新 `knowledge/`（Bot 自动提交）、知识库查询 | 修改系统文件、管理权限 |
| **user** | 知识库查询 | 所有写操作 |

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
