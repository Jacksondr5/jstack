---
name: j5code
description: Load it before any work in a Jacksondr5/j5code checkout. Jackson's own rules for J5 Code, on merging, give-backs and live testing, which override that repository's defaults.
---

# Jackson's rules for J5 Code

These are Jackson's own rules as a maintainer. The repository's `AGENTS.md` keeps defaults for every contributor; where these differ, these win for his work.

## Merging and give-backs

- **In J5 Code, code is your call; docs and UI are mine.** Open PRs for work I've given you without asking first. Review and merge code-only PRs yourself when CI is green and the review is sound. When a PR changes docs (`docs/`, register entries, agent-facing text) or UI, it needs my OK on those parts: tell your Director, who will message me with the PR link. If you have no Director and I'm not in the thread with you, send the PR link to my J5 Inbox. The doc diff and the before/after shots go on the PR, not in the thread. Nits I agree with go on the approval as asks; don't hold the merge or start another review for them.
- **Send upstream issues you find to the Giveback dispatcher, with the evidence.** Don't prepare or post give-backs yourself.

## Live testing

You are authorized to live-test without asking me first. This overrides the repository's rules to ask before using a browser and to live-test only upon request.

- Start a test server with the repository's `j5-live-test` skill, and drive it through the Browser panel.
- You are authorized to use Claude Haiku and GPT Luna for live test turns. Ask me before using a more expensive model. Cost is not a concern. Use real turns whenever they help. This overrides the repository's guidance to use the `Replay (dev)` provider unless a change depends on provider behaviour. Never avoid a real Haiku or Luna turn to save money.
- Have a Peer Agent in its own worktree do the testing, as the skill describes. It may start its own test server.
- Testing decisions are yours to make: how much to test, what to re-test, and what to leave as not exercised. Record each call and its reason in the PR. You don't need sign-off from me or from another agent unless I ask for it.

Never test against my live install.

## Mobile live testing

- **Every Simulator or emulator session needs a grant from the Mobile Simulator Orchestrator** (in the "No project" project; find it with `j5_list_participants`). Ask it before any rig setup, use only the device it grants, and send it a release report after `device_close` and stopping Metro and servers.
- Builders never run device commands on a rig slot. Prove rig changes with isolated fakes, and ask for a tester when a live check is needed. Never assume a device from a slot number.
- Leave network-off steps out of my phone checkpoints. Testers cover offline paths.
