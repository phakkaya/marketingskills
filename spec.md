# Marketing Skills — Agent Skills repo for AI-native marketing workflows

## Architecture

- **Skills** (`skills/*/SKILL.md`) — cross-agent instruction files following the Agent Skills spec (agentskills.io). Each skill is a directory with required YAML frontmatter (`name`, `description`) and optional `references/`, `scripts/`, `assets/` subdirs.
- **CLI tools** (`tools/clis/*.js`) — 51 zero-dependency Node.js scripts (Node 18+). One file = one tool.
- **Composio layer** (`tools/composio/`) — integration mapping for OAuth-heavy platforms (HubSpot, Salesforce, Meta Ads, LinkedIn Ads, Google Sheets, Slack).
- **Tool integrations** (`tools/integrations/`) — per-tool API guides (ga4, stripe, mailchimp, google-ads, resend, zapier, zoominfo, clay, supermetrics, coupler, outreach, crossbeam, introw, composio, rewardful, etc.).
- **Plugin manifest** (`.claude-plugin/marketplace.json`) — Claude Code plugin marketplace entry.
- **Tool registry** (`tools/REGISTRY.md`) — index of all tools with capabilities.
- **Versions** (`VERSIONS.md`) — skill version tracking. Fetched once per session to detect updates.

No build step. Skills are content-only; CLI tools verified with `node --check`.

## Done

- v1.5.0 shipped: customer-research skill, Nitrosend integration, Resend CLI, Firehose, Introw, Claude Code dynamic injection docs (commit 7c8c087).
- Claude Code `!`command`` injection pattern documented in AGENTS.md — auto-injects `.agents/product-marketing-context.md` into skills at load time.
- Composio integration layer added for platforms without native MCP servers.
- 51 CLI tools in `tools/clis/`.
- Plugin marketplace manifest live at `.claude-plugin/marketplace.json`.
- Conventional Commits + PR checklist enforced via AGENTS.md.

## Todo / Out of scope

- Add new skills as requested (follow naming conventions: lowercase, hyphens, match directory name).
- Cross-agent compatibility is a hard constraint — no Claude Code-only syntax (`` !`cmd` ``) in SKILL.md files; those go in `.claude/skills/` overrides only.
- Do NOT add build tooling, transpilation, or package.json dependencies to CLI tools — they must stay zero-dependency.

## Current state

Session started 2026-06-25. Added Operating Rules guardrails (Layer 1–3 of claude-md-setup skill) to AGENTS.md, created MEMORY.md and spec.md. No skill changes in this session yet.

Next concrete step: continue with whatever skill or tool work the user requests.
