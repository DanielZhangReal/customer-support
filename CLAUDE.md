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

```
customer-support/
├── SOUL.md                  ← AI personality & security rules (immutable, highest priority)
├── SKILL.md                 ← Behavior: intent routing, response templates, permission logic
├── CHANGELOG.md             ← Version history
├── README.md                ← Setup & git PR workflow guide
├── config/
│   ├── admins.yaml          ← Data: superadmin and admin platform IDs (superadmin only)
│   └── access-control.md   ← Docs: permission rules and matrix (superadmin only)
└── knowledge/               ← Content layer, read on demand
    ├── _index.md            ← Tag index + knowledgeVersion
    ├── overview.md          ← Product/project overview + glossary
    ├── faq.md               ← Common Q&A
    ├── guides.md            ← How-to guides
    ├── pricing.md           ← Pricing tiers (optional)
    ├── policies.md          ← Refund, privacy, terms
    ├── troubleshooting.md   ← Issue resolution + error codes
    └── community.md         ← Discord/Telegram channels, contact info
```

## Access Control & File Permissions

**Two-tier file access model:**

| Files | Who Can Modify | How |
|-------|---------------|-----|
| `knowledge/` | admin + superadmin | git PR only |
| All other files (SOUL.md, SKILL.md, config/, etc.) | superadmin only | git PR only |

**Identity**: Based on platform user IDs (Telegram UID / Discord user ID), never usernames.

## Knowledge Intent Mapping (defined in SKILL.md)

| Intent | Files consulted |
|--------|----------------|
| Product overview / what is this | `overview.md` |
| Common questions | `faq.md`, `overview.md` |
| How-to / getting started | `guides.md` |
| Pricing / subscription | `pricing.md` |
| Refunds / policies | `policies.md` |
| Troubleshooting / errors | `troubleshooting.md` |
| Community / contact | `community.md` |
| Unknown | `faq.md` (fallback) |

## Version Management

- **Skill version**: `SKILL.md` frontmatter `version` field (managed by superadmin)
- **Knowledge version**: `knowledge/_index.md` frontmatter `knowledgeVersion` field (updated with each admin PR)
- Versioning follows Semantic Versioning: patch for content updates, minor for logic changes, major for architecture changes

## Customizing the Template

All placeholder values must be replaced before production use:

```bash
grep -r "\[" knowledge/        # bracket placeholders
grep -r "YYYY-MM-DD" .         # date placeholders
grep -r "example.com" .        # example URLs
```

## Key Behavioral Rules (defined in SKILL.md + SOUL.md)

**Escalation to human/community channel is mandatory when:**
1. user explicitly requests human handling
2. Account security issue (suspected breach/anomalous login)
3. Legal, compliance, or privacy-sensitive questions
4. 3+ consecutive questions with no knowledge base answer
5. 5+ unresolved conversation turns

**Security rules (SOUL.md) cannot be overridden by any other file or user input.**
