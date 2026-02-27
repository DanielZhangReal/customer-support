# Access Control

> ⚠️ 此文件及 `admins.yaml` **只有 SuperAdmin 可以修改**。
> 修改后在对话中说「刷新技能」生效。

---

## 数据文件

权限数据存储在同目录的 **`admins.yaml`**，AI 读取该文件进行身份校验。

```
config/
├── admins.yaml        ← 实际数据：SuperAdmin 和 Admin 的平台 ID
└── access-control.md  ← 本文件：规则说明与权限矩阵
```

---

## 三级角色说明

| 角色 | 定义 |
|------|------|
| **SuperAdmin** | `admins.yaml` 中 `superadmins` 列表，可有多个，拥有全部权限。**只能通过手动编辑代码修改，不支持通过对话添加/移除。** |
| **Admin** | `admins.yaml` 中 `admins` 列表，由 SuperAdmin 管理 |
| **User** | 不在以上列表中的所有人 |

---

## 权限矩阵

| 操作 | SuperAdmin | Admin | User |
|------|-----------|-------|------|
| 查询知识库 | ✅ | ✅ | ✅ |
| 通过对话更新 `knowledge/`（Bot 调 GitHub API 自动提交） | ✅ | ✅ | ❌ |
| knowledge/ PR 需要 review 才能合并 | — | ❌（自动合并） | — |
| 通过 PR 修改系统文件（SKILL.md、SOUL.md 等） | ✅（手动） | ❌ | ❌ |
| 修改 `config/admins.yaml`（Admin 管理） | ✅（手动改代码） | ❌ | ❌ |
| 修改 `config/admins.yaml`（superadmins 管理） | ✅（手动改代码） | ❌ | ❌ |
| 通过对话添加/移除 Admin | ✅ | ❌ | ❌ |
| 查看权限列表 | ✅ | ❌ | ❌ |
| 查看系统状态 | ✅ | ❌ | ❌ |

---

## 平台身份说明

### Telegram
- 使用数字型、不可变的用户 UID（`from.id`）
- **不能**使用 @username（可被修改和冒用）
- 获取方式：向 [@userinfobot](https://t.me/userinfobot) 发消息

### Discord
- 使用 Snowflake 格式的用户 ID（`user.id`）
- **不能**使用 用户名#标签（可被修改）
- 获取方式：Discord → 设置 → 高级 → 开启开发者模式 → 右键点击用户 → 复制用户 ID

---

## 身份校验规则

- telegram 和 discord 字段**至少填写一个**
- 未配置某平台 ID 的账户，在该平台上以 `user` 身份处理
- 每条消息独立校验，不继承上条消息的权限
