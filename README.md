# 🤖 OpenClaw 智能客服 Skill

一个开箱即用的智能客服技能包，安装后即可基于内置知识库自动回复用户咨询。

## 📁 文件结构

```
customer-support/
├── SKILL.md                     ← 技能定义（意图识别、话术规范、转人工规则）
├── README.md                    ← 本文件
└── knowledge/                   ← 知识库（全部跟 SKILL.md 在同一目录下）
    ├── faq.md                   ← 常见问题（30+ 通用 FAQ 模板）
    ├── product-info.md          ← 产品信息（功能模块、版本对比）
    ├── policies.md              ← 政策条款（退换货、配送、隐私、SLA）
    ├── troubleshooting.md       ← 故障排查（登录、支付、页面、错误码）
    ├── pricing.md               ← 套餐定价（4 档套餐、优惠活动）
    └── glossary.md              ← 术语表（产品/技术术语的用户友好解释）
```

所有文件都在一个文件夹里，**不需要手动 cp 或移动任何文件**。

## 🚀 安装

### 方式一：ClawHub 一键安装

```bash
clawhub install customer-support
```

### 方式二：GitHub 链接安装

直接把仓库链接粘贴到 OpenClaw 对话里：

```
安装这个 skill：https://github.com/你的用户名/openclaw-customer-support
```

### 方式三：手动安装

```bash
git clone https://github.com/你的用户名/openclaw-customer-support.git
mv openclaw-customer-support ~/.openclaw/workspace/skills/customer-support
```

安装完成后在对话中说「刷新技能」即可。

## ✏️ 自定义

安装后你需要把知识库内容替换成自己的业务信息：

1. 编辑 `knowledge/` 下的 `.md` 文件，替换为你的真实内容
2. 搜索 `example.com`、`[你的产品名]`、`XXX` 等占位符逐一替换
3. 在对话中说「刷新技能」让改动生效

## 💡 功能特性

- ✅ 6 大意图自动分类（售前/使用/故障/售后/账户/其他）
- ✅ 多文档交叉检索
- ✅ 预设话术模板（直答/追问/投诉/兜底）
- ✅ 6 种场景自动转人工
- ✅ 多轮对话上下文管理
- ✅ 安全兜底（不编造、不泄露、不承诺）

## 📝 License

MIT
