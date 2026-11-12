---
name: t3code-giveback
description: Load it before any work in a pingdotgg/t3code checkout, and whenever a task names a GB-<n> item
---

# T3 Code upstream: giveback work

This skill covers the context you need to perform giveback work from J5 Code to T3 Code.  Only use it if you're specifically working on a giveback item.

## Where you are

- **J5 Code** (`Jacksondr5/j5code`, J5 project "J5 Code") is Jackson's public fork of T3 Code. Building on T3 turns up upstream bugs and gaps. A **giveback** is one of those, offered back to upstream: as a PR, an issue, a comment or an Ideas discussion.
- **This project** ("T3 Code upstream", `pingdotgg/t3code`) is a clean upstream clone and the home for giveback work. Its `AGENTS.md` and `CONTRIBUTING.md` are upstream's and they bind every change you make here. You cannot edit them, and nothing J5-specific gets committed in this repo.
- **The giveback dashboard** (https://j5-giveback.lakebed.app) is the source of truth for every item. Read [dashboard.md](dashboard.md) before you read or write item state.

## Standing rules

1. **Nothing goes upstream without Jackson's approval of the exact text.** This covers PR bodies, issues, comments, discussion posts, replies to bots or maintainers, PR description edits, and pushes that change a PR's meaning. Show the full final text and wait. Approval covers that version only. Only a purely mechanical rebase may be pushed without asking. Upstream maintainers merge; Jackson never does.
2. **Sign every public post with the model that wrote it**: `_[your model], an AI agent, on Jackson's behalf._`, with your real model's name. A PR also names the harness on its closing line. Write in the first person as Jackson ("I", not "we"). Never call their repo "upstream" in a post: their branch is `main`.
3. **Post only what adds something.** No "+1" comments (a 👍 reaction is fine when Jackson agrees). Never review or comment on other people's PRs unasked. Agent-written work done on a person's behalf is welcome there, as long as it stays low-volume and approved.
4. **Check before you claim.** Validate against current upstream `main` and record the SHA. Search issues, PRs (open *and* closed, by file and area, not only by linked issue) and discussions for duplicates. This repo sees a lot of traffic, so there's often activity on our giveback items. Reproduce live where you can, and don't rest a claim on a code read, your own or a subagent's. Re-validate anything that has sat for days.
5. **Evidence comes from T3's own build**, on an isolated upstream dev server (upstream's `test-t3-app` / `test-t3-mobile` skills). Live turns use Claude Haiku or GPT Luna. If a state can't be reached, say so in the post.
6. **Keep the dashboard current in the same session** as the change it records: stage, links, asks, events. Another agent picks up from what the dashboard says, not from your memory.

## Asking Jackson

- Name items by ID and title together: "GB-2, Steering could explain itself better".
- Lead with what needs him. Give each decision a bold **Decision needed:** line, lettered options, and your recommendation. Put one-line summaries of quiet events after it. Tell him about a merge or close once.
- Open a dashboard ask for every decision that waits on him, and close it when he answers (see [dashboard.md](dashboard.md)).
- He decides: whether to give an item back and by which route, every upstream text, fixes for bot findings before they are pushed, bug versus security report, stack versus wait, trade-offs, and moves of our items into a triage board's close or noise bucket. You decide how to validate, test, build and review.
- A bug in J5's own code goes to a `Jacksondr5/j5code` issue (`gh issue create -R Jacksondr5/j5code`). It is not fixed from here.

## The lifecycle

Work an item through [contributing.md](contributing.md): validate, recommend a route, prepare the branch and evidence, get the text approved, post, babysit, and close it out with J5's follow-up. Read it before you prepare or post anything.
