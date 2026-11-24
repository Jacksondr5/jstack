---
name: outer-loop
description: Use the Outer Loop Hub through the ol CLI. Load it when your instructions say to use Outer Loop.
---

<!-- Approved by Jackson (level D, #67, #71; items 5 and 8 revised 2026-11-23). Content is piece 8's skill outline (08-directors-and-cos.md), what he decided, plus the CLI commands. Source of truth: Jacksondr5/outer-loop agents/skills/outer-loop/SKILL.md; this is the installed copy. -->

# Outer Loop

1. **The Hub is Jackson's attention, and the truth about it.** Re-read before acting; he changes things from other places.
2. **Only what involves him goes in.** Your backlog stays in GitHub. No status reports, no "no change" items.
3. **Writing an ask:**
   - Two plain sentences of context.
   - 1–3 answers, each saying what it does. The first answer isn't always yes.
   - A suggestion only if you have one.
   - The right level, and whether it's reversible.
   - A deadline (required for B).
   - For replies: the draft, and a one-line summary of what they said.
   - Link the issue.
4. **Tasks for Jackson:** a plain action, a "why", a link to where the work starts, and a first step when it's hard to start.
5. **Deadlines:** when the tick says an ask's deadline has arrived, act on the default, then `close-on-default`. If you missed it, say so and handle it case by case. An ask's default is Jackson's own pre-approval: when the tick says the default time has come, act on it.
6. **Relaying Jackson's words:** `--for-jackson` only for what he actually said.
7. **Retries and refusals:** reuse the request id when retrying. A refusal says why, so don't work around it.
8. **Drafts you post as yourself:** when the tick says a post is pending, read the draft (`ol item <id> --json`), check the destination in case it already went out, post it if not, then `ol ask mark-posted <id>`. If `mark-posted` says it was already posted, don't post again. *(Decided 2026-11-23, piece 1 decision 7.)*

## Commands

`ol help <command>` gives the detail. Add `--json` to see every field. Every write takes `--request-id` for retries.

| Need | Command |
|---|---|
| What Jackson is on | `ol now` |
| Your domain's items, or one item with history | `ol items --domain <slug>` · `ol item <id>` |
| What changed since you last looked | `ol changes-since <seq>` |
| What's past due | `ol overdue --domain <slug>` |
| File an ask, task or know item | `ol item file --kind ask\|task\|know …` |
| Change an open item | `ol item update <id> --version <n> …` |
| Close an item | `ol item done <id>` · `ol item drop <id>` |
| Close an ask at its deadline | `ol ask close-on-default <id>` |
| Confirm a draft you posted | `ol ask mark-posted <id>` |
| Act on Jackson's words | add `--for-jackson` |
| Ask Jackson to bind this thread to your role (first time) | `ol role request <role>` |
