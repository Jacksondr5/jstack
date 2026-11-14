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

## One agent per item

Each open item has its own agent, in its own thread in this project. The thread is titled with the item's ID and title: "GB-30 · Merge back tooltip". Jackson works each item by talking to its agent.

**If you are an item's agent:**

- You own that one item, from where it stands to its close-out. Start from your hand-off and from `giveback show GB-<n>`.
- **Record yourself on the item before other work**: `private.agentName` (your thread's title) and `private.agentAddress` (your participant id, the row marked `self` in `j5_list_participants`). The watcher and the dispatcher find you through these fields.
- Work only your own item. What you learn about another item goes to its agent (`giveback agents` lists them). A new candidate, or an item with no agent, goes to the dispatcher.
- The watcher messages you when your item's upstream thread has new activity. Act on it as [contributing.md](contributing.md) step 6 says.
- When the item closes, finish the close-out, tell Jackson once, and stop. Leave your name on the item.

**The dispatcher** is one agent, in the thread titled "Giveback dispatcher". `giveback agents` prints its address. It does not work items. It:

- takes in a new candidate: creates the item, then starts its agent;
- starts an agent for any open item that has none. `giveback agents` shows these as unassigned. Start nothing while that list says it is incomplete;
- runs the daily triage-board scan and sends each change to the item's agent;
- receives the watcher's messages for items that have no agent;
- checks that the timers ran, that a backup exists, and that every recorded agent still answers.

To start an item's agent, the dispatcher uses `j5_spawn_agent` in this project, with Claude Opus 5.5, the thread title above, and a hand-off as the brief. The hand-off names the item by ID and title, and says where it stands, what waits on Jackson, where its branch, worktree and draft texts are, and what to do next. Local paths belong in the hand-off, never on the dashboard.

## Asking Jackson

- Name items by ID and title together: "GB-2, Steering could explain itself better".
- Lead with what needs him. Give each decision a bold **Decision needed:** line, lettered options, and your recommendation. Put one-line summaries of quiet events after it. Tell him about a merge or close once.
- Open a dashboard ask for every decision that waits on him, and close it when he answers (see [dashboard.md](dashboard.md)).
- He decides: whether to give an item back and by which route, every upstream text, fixes for bot findings before they are pushed, bug versus security report, stack versus wait, trade-offs, and moves of our items into a triage board's close or noise bucket. You decide how to validate, test, build and review.
- A bug in J5's own code goes to a `Jacksondr5/j5code` issue (`gh issue create -R Jacksondr5/j5code`). It is not fixed from here.

## The lifecycle

Work an item through [contributing.md](contributing.md): validate, recommend a route, prepare the branch and evidence, get the text approved, post, babysit, and close it out with J5's follow-up. Read it before you prepare or post anything.
