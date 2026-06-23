---
name: management-talk
description: Rewrite engineer-to-engineer content for engineering-org leadership (VPs, directors, PMs, release managers, execs in an engineering-savvy company) and shape it for the channel it is going to — JIRA comment, Slack post, async standup line, email, or meeting talking-points. Trigger when the user asks to write/rewrite for management / exec / VP / director / PM / release manager, asks for an "executive summary / leadership update / status update", says "make this less technical / less jargony", or asks for a slack / email / standup / meeting version of work originally written engineer-to-engineer.
---

# Management Talk

Same audience and translation rules as a written status report, but **shaped for the channel** — JIRA comment, Slack post, async standup, email, or meeting talking-points. The audience reads code names but not code. The channel decides the length, formatting, and how much structure to leave on the page.

## Audience — what "engineering-org leadership" means

Engineering-savvy non-engineers: VPs, directors, PMs, release managers, execs in companies that ship technical products. They read product/framework names and cross-reference ticket keys and PRs. They do not read code.

They want: *what's the state, what does it mean for customers, who owns it, what's next.* They do not want: how the bug works at the function level.

This is **not** for marketing, finance, customer-facing, or true ELI5 audiences — those need a different rewrite. Flag and confirm before producing one.

## Tone

**Keep.** Product names, framework names, team-owned component names, ticket keys, PR numbers, customer/workload identifiers. These are the bridge between engineering and leadership tracking.

**Strip.** Function names, file paths, struct fields, commit SHAs, code expressions, env var names, line numbers, internal data-structure jargon. None of this is actionable to the audience.

**Translate.** Mechanism into one or two sentences of plain-English cause-and-effect. Translate without lying — a race stays a race; a regression stays a regression.

**Don't over-strip.** Engineering-org leadership reads concept-level technical vocabulary fluently — *race condition, synchronization, uninitialized buffer, fast-path, workaround, registration, queue, driver, kernel* (in the GPU sense). The line is between *concept exists and matters here* (keep) and *here's the function/struct/file/SHA* (strip).

**Bias toward** active voice, concrete subjects, short paragraphs.

**Avoid:**
- Hedging that isn't really hedging (*"we believe," "appears to," "may have"*). State it or don't.
- Re-stating the obvious for thoroughness.
- Telling leadership how to do their job. Give them the facts; they decide.

## Channel shapes

### JIRA comment / written status report

Full structured block. Bolded section labels. Easy to scan from the ticket page.

Building blocks (use as many as fit):

- **Status / TL;DR.** One bolded line. Reader can stop here and have the right answer.
- **Impact.** Who's affected, how badly, what they see. Customer / workload / product terms, not test-suite terms.
- **What broke.** Short paragraph. Plain-English mechanism, one level of why, no code identifiers.
- **Why now / how it slipped through.** Optional. Include when leadership will ask anyway.
- **Owner.** Person + team + their PR/branch/ticket artifact. One link, not five.
- **Next steps.** Concrete, near-term, ordered.
- **Workaround / mitigation.** If customers are hitting it now, what can they do today? One sentence.
- **Risk.** Optional. Real risks only.

### Slack — channel post or DM

Single message, no walls of text.

- One **bolded TL;DR** as the first line.
- 2–4 short bullets underneath: impact, owner+link, next step. Drop blocks that don't apply.
- One link, embedded inline. Not a link wall.
- No greeting, no signoff.
- If it's a **thread reply** rather than a new post, lose the TL;DR — just lead with the answer.

Length target: under ~80 words for a top-level post; under ~40 for a thread reply.

### Async standup note

The audience scans 10 of these in 30 seconds. Front-load the verb.

- 1–3 lines, max.
- Pattern: *"\<state\> \<thing\>. \<owner if not me\>. \<next\>."*
- No bullets, no bolded labels. The format **is** the sentence.

### Email — internal exec / cross-team

- **Subject:** the TL;DR rewritten as a noun phrase.
- **Greeting:** match the recipient register.
- **Body:** the JIRA-comment shape, but as flowing paragraphs. Two or three paragraphs is plenty.
- **Sign off** with the next decision point that needs the recipient's attention, if any.

### Meeting talking-points

You're going to *say* this, not show it.

- Bullet list, max one short clause per bullet.
- Order is the order you'll speak in.
- Skip prose — just the key facts you want to reference out loud.

## Output flow

1. **Confirm the channel** if it's not stated.
2. **Produce the draft** as a single chat block, formatted as the channel would render it.
3. **Ask where it goes** — default is print-only; the user copies it.
4. **One iteration is normal, three is a smell.**

## Rules

- **Never invent facts** to make the rewrite cleaner. If the source says "root cause unknown," the rewrite says "root cause unknown."
- **Never strip a ticket key, PR number, or customer/workload name** during de-jargoning.
- **Never invent owners.** If the source doesn't name one, ask the user.
- **Get sign-off before posting to any issue tracker.**
- **Never post to Slack, email, or any non-JIRA channel from this skill.** Hand the draft to the user; they post it.
- **Stay out of advocacy.** This skill produces a status update, not a recommendation.
