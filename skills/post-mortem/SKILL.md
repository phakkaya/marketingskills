---
name: post-mortem
description: Write the canonical engineering record of a fixed bug — root cause, mechanism, fix, validation, and how it slipped through. Engineer-audience, code identifiers welcome. Use after a debug session lands a fix, before closing the ticket. Trigger on /post-mortem, when the user says "write the post-mortem / postmortem / RCA / root cause analysis", "document this fix", "write up the root cause", "close out this bug with a writeup", or hands you a fixed-and-validated bug and asks for the writeup.
---

# Post-mortem

The canonical engineering record of a bug fix. Written **after** debugging lands a real fix, **for** other engineers (and future-you, who will have forgotten everything in 6 months). Code identifiers are welcome here — this is the artifact that lets the next person recover the mental model fast.

## When NOT to use

- **Bug not fixed yet, or fix not validated.** A post-mortem of a hypothesis is misleading. Refuse and tell the user what's missing.
- **Customer-visible outage / incident.** Those need a separate incident report (timeline, blast radius, paging history, comms). This skill is bug-fix scope. Flag and confirm before producing one.
- **Trivial fix** (typo, obvious one-liner). The PR description is the record. Don't manufacture ceremony.

## Required inputs — refuse to draft without these

Before writing a single line, confirm all four. If any are missing, list what's missing and stop:

- [ ] **Reliable repro** exists (not "happens sometimes" — a deterministic or high-rate-flake repro the next person can run).
- [ ] **Root cause is known** (the mechanism is identified, not a hypothesis).
- [ ] **Fix is identified** (PR / commit / branch pointer).
- [ ] **Fix is validated** (the original repro now passes; the customer workload / failing test now succeeds).

## Structure

Use these blocks in this order. **Summary, Root cause, Fix, and Validation are mandatory.**

### 1. Summary _(mandatory)_
One paragraph. What broke, in user/workload terms. What fixed it, in one sentence. A reader who stops here should have the right answer.

### 2. Symptom
What was actually observed. Test output, error message, log line, perf number, customer report. Concrete identifiers — don't paraphrase the failure mode.

### 3. Root cause _(mandatory)_
The actual bug mechanism. **Code identifiers welcome and expected** — function names, file paths, struct fields, branch conditions, commit SHAs of the offending change. Walk the cause chain end-to-end.

### 4. Why it produced the symptom
Link the root cause to the symptom. Often non-obvious — the bug is in one place but the visible failure appears hours later. Walk the chain so a reader who only knows the symptom can connect it back to the cause.

### 5. Fix _(mandatory)_
What changed and **why this change addresses the root cause** rather than hiding the symptom. Link to PR / commit. If a previous fix attempt papered over the symptom, name it and explain what was wrong with it.

### 6. How it was found
Short. The debugging path:
- What repro made it deterministic.
- What tools cracked it (debugger, source tracing, knob enumeration, in-code instrumentation).
- Hypotheses tried and rejected, with the one-line reason each was rejected.
- The single experiment that confirmed the cause.

### 7. Why it slipped through
What allowed this bug to reach the branch / release / customer. Pick the real reason:
- CI gap (no test exercises this path / configuration).
- Latent code (correct when written, broken by a later change in a different file).
- Workload gap (no real workload reached this code path until now).
- Incomplete prior fix.
- Review miss.

**Blameless** — describe the gap, not the person.

### 8. Validation _(mandatory)_
How we know the fix works. Concrete:
- Original failing test now passes (test name, link).
- Customer workload now completes.
- Perf regression resolved (number before, number after).

If you only validated one configuration, say so explicitly. Don't imply broader coverage than you actually have.

### 9. Action items / follow-ups
Concrete next-steps that aren't in the fix PR itself. Each item: what + owner + tracking artifact.

If there are no action items, write *"None — the fix is sufficient and no class-of-bug follow-up is warranted."* Don't manufacture action items to look thorough.

## Tone

This is engineer-to-engineer:

- **Code identifiers are first-class.** Keep them. The whole point is that future engineers can grep their way back to the change.
- **Mechanism over narrative.** Walk the actual cause chain.
- **Active voice, concrete subjects, short paragraphs.**
- **No hedging.** "We believe" / "appears to" / "may have" — drop. State it or don't write it.
- **Blameless.** Describe the bug, the gap, and the fix. Never "X should have caught this."

## Rules

- **Refuse to draft without all four required inputs.** A post-mortem of a hypothesis is worse than no post-mortem.
- **Never invent root cause, owner, validation runs, or action items.** If a section's facts aren't there, ask.
- **Never strip code identifiers** in the engineering record.
- **Get sign-off before posting to issue trackers.** Print-only output needs no approval.
- **One iteration is normal, three is a smell.** If the user is still revising on the third pass, ask what specific section is wrong.
