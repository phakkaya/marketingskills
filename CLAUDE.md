# CLAUDE.md

Guidelines for Claude Code working in this repository.

> Cross-agent guidelines live in `AGENTS.md`. This file adds Claude Code-specific context and keeps counts/structure accurate for the actual repo state.

## Repository Overview

This repository contains **Agent Skills** for AI agents following the [Agent Skills specification](https://agentskills.io/specification.md). Skills install to `.agents/skills/` (the cross-agent standard). The repo also serves as a **Claude Code plugin marketplace** via `.claude-plugin/marketplace.json`.

- **Name**: Marketing Skills
- **GitHub**: [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills)
- **Creator**: Corey Haines
- **License**: MIT

## Repository Structure

```
marketingskills/
├── .claude-plugin/
│   └── marketplace.json        # Claude Code plugin marketplace manifest (auto-synced by CI)
├── .github/
│   ├── scripts/
│   │   └── sync-skills.js      # Auto-syncs marketplace.json + README on push to main
│   ├── workflows/
│   │   ├── sync-skills.yml     # Triggered on skills/** changes pushed to main
│   │   └── validate-skill.yml  # Validates changed SKILL.md files on PR/push to main
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE/
├── skills/                     # 34 Agent Skills
│   └── skill-name/
│       ├── SKILL.md            # Required — main instructions (<500 lines)
│       ├── evals/
│       │   └── evals.json      # Quality evals (197 total across all skills)
│       ├── references/         # Optional — detailed docs loaded on demand
│       ├── scripts/            # Optional — executable code
│       └── assets/             # Optional — templates, data files
├── tools/
│   ├── clis/                   # 61 zero-dependency Node.js CLI tools (Node 18+)
│   ├── composio/               # Composio integration layer (quick start + toolkit mapping)
│   ├── integrations/           # API integration guides per tool
│   └── REGISTRY.md             # Tool index with capabilities
├── AGENTS.md                   # Cross-agent guidelines (Codex, Cursor, Windsurf, etc.)
├── CLAUDE.md                   # This file — Claude Code-specific guidelines
├── CONTRIBUTING.md
├── LICENSE
├── README.md                   # Skills table auto-updated by sync-skills.yml
├── VERSIONS.md                 # Skill version tracking for update checks
├── validate-skills.sh          # Local validation script (no external dependencies)
└── validate-skills-official.sh # Validation via official skills-ref Python library
```

## Build / Lint / Test Commands

**Skills** are content-only (no build step). Two validation options:

```bash
# Fast local validation — no dependencies required
bash validate-skills.sh

# Official validation via skills-ref library (clones agentskills/agentskills on first run)
bash validate-skills-official.sh
```

Manual checks for each skill:
- YAML frontmatter present and valid
- `name` matches directory name exactly
- `name` is 1-64 chars, lowercase alphanumeric + hyphens only
- `description` is 1-1024 characters with trigger phrases
- `SKILL.md` is under 500 lines

**CLI tools** (`tools/clis/*.js`) are zero-dependency Node.js scripts (Node 18+):
```bash
node --check tools/clis/<name>.js          # Syntax check
node tools/clis/<name>.js                  # Show usage (no args = help)
node tools/clis/<name>.js <cmd> --dry-run  # Preview request without sending
```

## CI / GitHub Actions

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `validate-skill.yml` | PR or push to `main` touching `**/SKILL.md` | Validates each changed skill via `Flash-Brew-Digital/validate-skill@v1` action |
| `sync-skills.yml` | Push to `main` touching `skills/**` | Runs `sync-skills.js` to rebuild `marketplace.json` + README skills table, then auto-commits as Coreybot |

**Important**: Do not manually edit the `<!-- SKILLS:START -->…<!-- SKILLS:END -->` block in `README.md` or `.claude-plugin/marketplace.json` — `sync-skills.yml` auto-generates and overwrites them on every push to main.

## Agent Skills Specification

Skills follow the [Agent Skills spec](https://agentskills.io/specification.md).

### Required Frontmatter

```yaml
---
name: skill-name
description: When the user wants to... Use when the user says "X." For related task, see other-skill.
---
```

### Frontmatter Field Constraints

| Field         | Required | Constraints                                                      |
|---------------|----------|------------------------------------------------------------------|
| `name`        | Yes      | 1-64 chars, lowercase `a-z`, numbers, hyphens. Must match dir.   |
| `description` | Yes      | 1-1024 chars. Describe what it does and when to use it.          |
| `license`     | No       | License name (default: MIT)                                      |
| `metadata`    | No       | Key-value pairs — version goes here, not at top level            |

### Name Field Rules

- Lowercase letters, numbers, and hyphens only
- Cannot start or end with a hyphen
- No consecutive hyphens (`--`)
- Must match parent directory name exactly

**Valid**: `page-cro`, `email-sequence`, `ab-test-setup`
**Invalid**: `Page-CRO`, `-page`, `page--cro`

## Current Skills (34)

`product-marketing-context` is the foundation — every other skill reads `.agents/product-marketing-context.md` before proceeding.

| Category | Skills |
|----------|--------|
| Conversion Optimization | `page-cro`, `signup-flow-cro`, `onboarding-cro`, `form-cro`, `popup-cro`, `paywall-upgrade-cro` |
| Content & Copy | `copywriting`, `copy-editing`, `cold-email`, `email-sequence`, `social-content` |
| SEO & Discovery | `seo-audit`, `ai-seo`, `programmatic-seo`, `site-architecture`, `competitor-alternatives`, `schema-markup` |
| Paid & Distribution | `paid-ads`, `ad-creative` |
| Measurement & Testing | `analytics-tracking`, `ab-test-setup` |
| Retention | `churn-prevention` |
| Growth Engineering | `free-tool-strategy`, `referral-program` |
| Strategy & Monetization | `marketing-ideas`, `marketing-psychology`, `launch-strategy`, `pricing-strategy`, `content-strategy` |
| Sales & RevOps | `revops`, `sales-enablement` |
| Research & Context | `customer-research`, `lead-magnets`, `product-marketing-context` |

## Writing Style Guidelines

### Structure

- Keep `SKILL.md` under 500 lines — move detail to `references/`
- Use H2 (`##`) for main sections, H3 (`###`) for subsections
- Bullet points and numbered lists over paragraphs
- Short paragraphs (2-4 sentences max)

### Tone

- Direct and instructional
- Second person ("You are a conversion rate optimization expert")
- Professional but approachable

### Formatting

- Bold (`**text**`) for key terms
- Code blocks for examples and templates
- Tables for reference data
- No excessive emojis

### Clarity Principles

- Clarity over cleverness; specific over vague
- Active voice over passive
- One idea per section

### Description Field Best Practices

The `description` is critical for skill discovery. Include:
1. What the skill does
2. When to use it (trigger phrases)
3. Related skills for scope boundaries

```yaml
description: When the user wants to optimize conversions on any marketing page. Use when
  the user says "CRO," "conversion rate optimization," "this page isn't converting."
  For signup flows, see signup-flow-cro.
```

## Claude Code Plugin

This repo serves as a Claude Code plugin marketplace. Skills are installed via:

```bash
/plugin marketplace add coreyhaines31/marketingskills
/plugin install marketing-skills
```

## Git Workflow

### Branch Naming

- New skills: `feature/skill-name`
- Improvements: `fix/skill-name-description`
- Documentation: `docs/description`

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat: add skill-name skill`
- `fix: improve clarity in page-cro`
- `docs: update README`

### Pull Request Checklist

- [ ] `name` matches directory name exactly
- [ ] `name` follows naming rules (lowercase, hyphens, no `--`)
- [ ] `description` is 1-1024 chars with trigger phrases
- [ ] `SKILL.md` is under 500 lines
- [ ] `bash validate-skills.sh` passes locally
- [ ] No sensitive data or credentials

## Tool Integrations

- **Tool discovery**: Read `tools/REGISTRY.md` for available tools and capabilities
- **Integration details**: `tools/integrations/{tool}.md` — API endpoints, auth, common operations
- **MCP-enabled tools**: ga4, stripe, mailchimp, google-ads, resend, zapier, zoominfo, clay, supermetrics, coupler, outreach, crossbeam, introw, composio
- **Composio**: MCP access to OAuth-heavy tools (HubSpot, Salesforce, Meta Ads, LinkedIn Ads, Google Sheets, Slack). See `tools/integrations/composio.md` and `tools/composio/marketing-tools.md`

Skill → tool mappings:
- `referral-program` → rewardful, tolt, dub-co, mention-me
- `analytics-tracking` → ga4, mixpanel, segment
- `email-sequence` → customer-io, mailchimp, resend
- `paid-ads` → google-ads, meta-ads, linkedin-ads
- `customer-research` → g2, trustpilot, dataforseo, semrush

## Checking for Updates

On first skill use each session:

1. Fetch `VERSIONS.md` from GitHub and compare skill versions against local files
2. Notify only if 2+ skills have updates or any skill has a major version bump:
   ```
   ---
   Skills update available: X marketing skills have updates.
   Say "update skills" to update automatically, or run `git pull` in your marketingskills folder.
   ```
3. If user says "update skills": run `git pull` in the marketingskills directory and confirm what changed.

## Claude Code-Specific Enhancements

These patterns are **Claude Code only** — do not add them to `SKILL.md` files, which must stay cross-agent compatible (Codex, Cursor, Windsurf, etc.). Apply them in your project's `.claude/skills/` overrides instead.

### Dynamic Content Injection with `` !`command` ``

Claude Code executes shell commands embedded in SKILL.md using `` !`command` `` syntax at skill invocation time — the model sees the output, not the instruction.

**Auto-inject product marketing context** (most useful pattern):

```markdown
Product context: !`cat .agents/product-marketing-context.md 2>/dev/null || echo "No product context file found — ask the user about their product before proceeding."`
```

**Other useful injections:**

```markdown
Today's date: !`date +%Y-%m-%d`
Current branch: !`git branch --show-current 2>/dev/null`
Recent commits: !`git log --oneline -5 2>/dev/null`
```

**Why this is Claude Code-only**: Other agents see the literal `` !`command` `` string, not the output.
