# Contributing a giveback to pingdotgg/t3code

How an item moves from candidate to closed. Dashboard stages are in brackets. Upstream's `CONTRIBUTING.md` on `main` is the policy. This page is how we meet it, plus the lessons that cost us something.

## What upstream accepts

Upstream rewrote `CONTRIBUTING.md` on 2026-11-11. Review time is their bottleneck, and they say most outside PRs will be closed. Read it on `main` before you recommend a route. In short:

- **Bug fixes only.**
- **3 open PRs per person.** Jackson is not vouched, so his 4th open PR is closed when it opens.

## 1. Validate [found → validating]

- Read the code on fresh upstream `main` (`git fetch origin`) and record the SHA in `lastValidatedSha` with a one-line `validationVerdict`.
- Search issues, PRs and discussions for duplicates. Include closed PRs, since GitHub hides commits pushed after a PR closes, and search PRs by file and area too, since some fixes link no issue. Read the PR that introduced the behaviour, in case it was a deliberate choice.
- Reproduce it live on an isolated upstream dev server.  Take advantage of all connected environments as needed.
- Possible verdicts:
  - still real: go to step 2;
  - covered by someone's open PR: `held`, with a `covering` link;
  - not a bug;
  - J5-only;
  - deliberate upstream design: Ideas material at most.

  An upstream choice that harms no upstream user is not a giveback.
- **Decide: bug or feature.** Use upstream's own test: a change to how the product behaves is a feature, even when the PR calls it a fix. Record the verdict in `changeKind`: `bug`, `behaviour_change` or `feature`. A behaviour change or a feature can become an Ideas discussion or be dropped. It never becomes a PR.
- **Related open items.** When you find an open upstream issue or PR related to your item, tell Jackson in your recommendation. He decides case by case what we do there, if anything.

## 2. Recommend a route [deciding, waiting on Jackson]

Open a `route_decision` ask with your recommendation:

- **Direct PR** (`direct_pr`): a tiny fix for an obvious bug. Upstream asks for no issue here. The PR's Scope section says why it needs none.
- **Issue first** (`issue_first`): the default for anything beyond a tiny fix. File the issue and prepare the PR (step 3), but open the PR only when a maintainer asks for it, or when nobody has picked the issue up after about a week and Jackson agrees. Upstream allows a larger fix to open with its issue linked. Waiting is still cheaper for us: maintainers have often fixed our issues themselves, which spends no PR slot. Never file an issue *alongside* a PR: it reads as two confusingly related items.
- **Comment** (`comment`): someone else's thread already covers it, and we have new evidence or an offer.
- **Ideas discussion** (`discussion`): a feature or an intentional behaviour change. A PR for one is closed unless a maintainer asked for it, and then the PR links the request. File it under Ideas, not Q&A. Calling a behaviour change a bug fix does not exempt it.
- **Hold / drop** (`hold`, `drop`): the value doesn't justify the cost, or it isn't ours to raise.

**Count the slot cost.** Any route that ends in a PR takes one of our 3 open-PR slots. Run `giveback capacity`, and say in the recommendation what is open and queued now, and why this item is worth a slot ahead of what is queued. Jackson orders the queue by value.

Security-shaped items: check https://t3.codes/security-policy, and keep the item `isPublic: false` until it is filed. 

## 3. Prepare [preparing]

- **Branch** from fresh `origin/main` in your J5 worktree, named `fix/<what-it-fixes>`. One underlying problem per PR.
- **Push** to J5's GitHub fork, which is the fork of t3code: `git push j5 HEAD:refs/heads/<branch>`. The `j5` remote is `https://github.com/Jacksondr5/j5code.git`; add it if it is missing. A builder subagent commits locally, and the agent that owns the item pushes. expo/expo-device-hub work uses `Jacksondr5/expo-device-hub`.
- **Tests:** focused tests that fail on `main` and pass on the branch, following upstream's AGENTS.md testing rules. Before posting, run `vp run check:changed` and `vp test run <files>`. Revert incidental lockfile churn, and keep dev homes and scratch out of the diff. Use fnm's Node, not an ad-hoc one.
- **Evidence:** UI changes need before and after captures on matched data, viewport and theme.
  - Images go on the `j5/evidence` branch of `Jacksondr5/j5code` under `upstream/<slug>/`, embedded as raw URLs. A re-capture gets a new file name, because raw URLs cache.
  - Videos go up as GitHub user-attachments (see Jackson's global AGENTS.md).
  - A peer tester on a different model usually captures them.
- **Review:** before posting non-trivial work, use the `council` skill to get a good review. Run the "not checked" items you can, instead of listing them.
- **Draft the text** into a J5 artifact (`j5_write_artifact`, e.g. `giveback/GB-12-pr.md`). Point the ask's `private.draftPointer` at it. The PR body follows upstream's template: Problem, Change, Scope, Verification. Scope links the issue, or says why a small fix needs none. Verification says how the bug was found, how the fix was checked and what was seen, and names what could not be checked. Then the signature, and the harness line.

## 4. Approve [approving, waiting on Jackson]

Open a `text_approval` ask carrying the exact text. When he approves an issue's PR in advance, record that as a *closed* `text_approval` ask whose resolution states the condition, and set `waitingOn: maintainer`.

## 5. Post [offered]

- Rebase onto the latest `origin/main` and re-run the focused tests. Before any push to the branch of a PR that is already open, check its labels: see the landing pipeline in step 6.
- **Check the posting limits first** (standing rule 7): `giveback capacity` shows the posts of the last 24 hours and 7 days, and the new PRs and issues, each beside its limit. It counts from GitHub, so it includes posts no agent recorded. If `posts.source` is `events`, GitHub could not be read and the counts may be low: check his recent activity on GitHub yourself before posting. If this post would pass a limit, wait, and tell Jackson if it cannot wait.
- **Then the open PR limit.** Run `gh pr list -R pingdotgg/t3code --author Jacksondr5 --state open`. If 3 are open, don't post: put the item in `held` with `waitingOn: pr_slot`, and tell the dispatcher. This is the one use of `held` that needs no `covering` or `base` link.
- **Waiting for a slot.** The dispatcher sets the item's `queueRank` in the order Jackson gives. A slot frees when one of our PRs merges or closes. The dispatcher learns that from the watcher, which tells it whenever one of our PRs closes or merges, and from its daily check. It then tells the agent of the first item in the queue, and that agent rebases, re-runs the focused tests, and opens its PR, or reopens the one the bot closed. Never open or reopen before the dispatcher says so.
- Open the PR: `gh pr create -R pingdotgg/t3code --head Jacksondr5:<branch>`. For an issue, use upstream's issue template.
- In the same session:
  - set the item's `primary` link (`ownership: ours`) and stage `offered`;
  - add a `posted` event with the post's URL. Every upstream post gets one: a PR, an issue, a comment, a discussion post, a reply. The dashboard counts them per week.

## 6. Babysit [offered]

- **Review bots count.** Macroscope comments on nearly every PR. Its approval is good news. "Needs Human Approval", or comments from it, mean we adjust the PR or answer its comments. CodeRabbit's actionable findings and Devin's reviews also count. For each finding: verify it against the source, prepare the fix on the branch, then bring Jackson the finding, your verdict and the change *before* you push or reply. False positives get a reasoned reply, once he approves it.
- **Failing checks** get the same treatment as bot findings.
- **A `needs-rebase` label** starts a 7-day clock, which the dashboard shows on the PR. Rebase within a day or two, unless the landing pipeline has the PR (below). A purely mechanical rebase needs no approval. Anything more goes to Jackson first.
- **A notice that the PR breaks a rule** (the `triage:closing` label) starts a 72-hour clock, which the dashboard also shows. Tell Jackson at once, with the rule it names, your reading of whether it is right, and the reply or push you propose. Either one, inside the 72 hours, sends the PR back for another look.
- **A closure** is never argued. If the reason can be fixed, bring Jackson the fix and a comment, or a focused replacement PR that links the old one.
- **A maintainer's question** goes to Jackson as a `maintainer_question` ask.
- **A maintainer's nod** on an offered fix: with text approved in advance, rebase, re-test, open the PR, then tell him. Without it, bring him the ready PR and wait. Speed matters: be ready to open a PR as soon as a maintainer agrees.
- **Someone else opens a PR** for our item: don't open ours. Compare their approach with ours, move the item to `held` with a `covering` link, and tell Jackson what you found. He decides whether we react or comment there.
- **Rebases:** never while the PR carries `takeover:in-progress` or `takeover:ready`. Otherwise force-push with lease only after re-checking the remote head, and only for a purely mechanical change. Additions go on as new commits. GitHub stacks can't cross forks, so wait for an outsider's base PR to merge rather than stacking on it.
- **Third-party noise** (cross-links, +1s from others): one line in your report, or nothing.

### Upstream's landing pipeline ("takeover")

Since about 2026-11-13 most outside PRs land through an automated pipeline that runs under a maintainer's account. This is what it did when we looked on 2026-11-15. It is new and nothing in their repository defines it, so expect it to change: trust the PR's labels over this page, and tell the dispatcher when what you see differs.

- `takeover:in-progress`: the pipeline merges `main` into our branch, may add commits, reviews the change and captures its own evidence. This takes about half an hour.
- Then one of three: `takeover:ready`, merged within about a minute; `takeover:needs-julius`, waiting on a maintainer's decision, usually with no comment; or `takeover:blocked`, with a comment that says why it stopped.
- It can also close a PR itself: when the bug did not reproduce without the change, when the PR is redundant or a duplicate, or when it was rebuilt as a maintainer's PR with our commits kept.
- Its comments end with a marker such as `<!-- factory:stopped-moved -->`. The watcher's message shows these as "pipeline comment", with the marker.
- Credit is kept: the squash commit's author is the contributor.

What we do:

- **While a PR carries `takeover:in-progress` or `takeover:ready`, never push to its branch,** not even a mechanical rebase, and do not reply on it. A push from anyone else stops the pipeline. Check the labels before any push: `gh pr view <n> -R pingdotgg/t3code --json labels`.
- **After `takeover:blocked`, a closure by the pipeline, or a pipeline comment that asks for steps:** bring Jackson the reason and a proposed reply or fix. He approves it before anything goes up.
- **`takeover:needs-julius` needs nothing from us.** The PR still holds one of our 3 slots until it merges or closes.
- **Keep the PR body's evidence complete** (step 3). The pipeline captures its own, but its most common stop is that it could not prove the fix, and our before and after captures and platform notes may be what lets it.
- The dashboard shows the label on the PR as GitHub has it.

## 7. Close out [closed]

- Set the outcome: `merged`, `fixed_by_maintainer_pr`, `taken_over`, `covered_by_contributor`, `fixed_upstream`, `closed_by_upstream`, `not_a_giveback`, `parked` or `dropped`. Stop watching a closed item.
- **J5 follow-up.** When the fix is upstream, J5 drops its own copy at its next upstream advance. That means retiring a FORK.md case, or a D-number in `docs/j5/product/upstream.md`, in the J5 Code source. Record it as `followUpState: owed` with a `followUpNote` naming what to retire. Tell the J5 advance tracker issue on `Jacksondr5/j5code`. That work happens in J5 Code, not here.
