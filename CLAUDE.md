# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is an **OpenClaw customer support skill package** — a documentation-only template, not a software project. There are no build steps, no tests, no dependencies, and no code to compile. All files are Markdown.

## Deployment

Install via the OpenClaw platform using one of three methods:

```bash
# ClawHub (package manager)
clawhub install customer-support

# Manual install
git clone <repo-url>
mv <repo-dir> ~/.openclaw/workspace/skills/customer-support
```

After any file edit, say `刷新技能` (refresh skill) in an OpenClaw conversation to apply changes.

## Architecture

`SKILL.md` is the AI system prompt. It defines the full behavioral contract: intent routing, response templates, escalation rules, and prohibited behaviors. The `knowledge/` directory is the content layer read on-demand during conversations.

**Intent → Knowledge mapping** (defined in `SKILL.md`):

| Intent | Files consulted |
|--------|----------------|
| 售前咨询 (pre-sales) | `product-info.md`, `pricing.md` |
| 使用指导 (usage) | `faq.md`, `product-info.md` |
| 故障排查 (troubleshooting) | `troubleshooting.md` |
| 售后服务 (after-sales) | `policies.md` |
| 账户问题 (account) | `faq.md` |
| 闲聊/其他 (other) | none |

## Customizing the Template

This is a generic template. All placeholder values must be replaced before production use. Locate them with:

```bash
grep -r "\[" knowledge/         # bracket placeholders like [你的产品名]
grep -r "XXX" knowledge/
grep -r "example\.com" .
```

Each knowledge file's scope:
- **`faq.md`** — 30+ Q&A pairs across 6 categories; replace with real FAQ content
- **`product-info.md`** — features, version comparison table, platform support
- **`policies.md`** — refund rules, shipping SLAs, privacy policy
- **`troubleshooting.md`** — step-by-step fixes and error code reference table
- **`pricing.md`** — 4 pricing tiers (free/basic/pro/enterprise) and promotions
- **`glossary.md`** — 40+ terms with user-friendly explanations

## Key Behavioral Rules (defined in SKILL.md)

**Escalation to human agent is mandatory when:**
1. User explicitly requests human agent
2. Refund amount exceeds ¥500
3. User expresses dissatisfaction 3+ consecutive times
4. Account security issue (suspected breach/anomalous login)
5. No relevant information in knowledge base
6. Legal, compliance, or privacy-sensitive questions

**Auto-escalation** also triggers after 5 unresolved conversation turns.
