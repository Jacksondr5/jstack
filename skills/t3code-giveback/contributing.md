# Contributing a giveback to pingdotgg/t3code

How an item moves from candidate to closed. Dashboard stages are in brackets. Upstream's `CONTRIBUTING.md` on `main` is the policy. This page is how we meet it, plus the lessons that cost us something.

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

## 2. Recommend a route [deciding, waiting on Jackson]

Open a `route_decision` ask with your recommendation:

- **Direct PR** (`direct_pr`): a very small, focused fix for an obvious bug. The PR explains why it qualifies. While the `CONTRIBUTING.md` seems to lead towards requiring an issue first, in practice most of our obvious fixes have gotten merged, so we can offer a PR more than you'd think.
- **Issue first** (`issue_first`): a substantial bug that needs maintainer triage. Prepare the PR anyway (step 3) so it can open the moment a maintainer agrees. Never file an issue *alongside* a PR: it reads as two confusingly related items.
- **Comment** (`comment`): someone else's thread already covers it, and we have new evidence or an offer.
- **Ideas discussion** (`discussion`): a feature or an intentional behaviour change, which needs explicit maintainer approval before any PR. File it under Ideas, not Q&A. Calling a behaviour change a bug fix does not exempt it.
- **Hold / drop** (`hold`, `drop`): the value doesn't justify the cost, or it isn't ours to raise.

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
- **Draft the text** into a J5 artifact (`j5_write_artifact`, e.g. `giveback/GB-12-pr.md`). Point the ask's `private.draftPointer` at it. The PR body follows upstream's template: Problem, Change, Scope and approval, Verification, Not checked. Then the signature, and the harness line.

## 4. Approve [approving, waiting on Jackson]

Open a `text_approval` ask carrying the exact text. When he approves an issue's PR in advance, record that as a *closed* `text_approval` ask whose resolution states the condition, and set `waitingOn: maintainer`.

## 5. Post [offered]

- Rebase onto the latest `origin/main` and re-run the focused tests.
- Open the PR: `gh pr create -R pingdotgg/t3code --head Jacksondr5:<branch>`. For an issue, use upstream's issue template.
- In the same session:
  - set the item's `primary` link (`ownership: ours`) and stage `offered`;
  - add an event;

## 6. Babysit [offered]

- **Review bots count.** Macroscope's approvability verdict gates upstream's deeper review, and "Needs Human Approval" is a gate only a maintainer can clear. CodeRabbit's actionable findings and Devin's reviews also count. For each finding: verify it against the source, prepare the fix on the branch, then bring Jackson the finding, your verdict and the change *before* you push or reply. False positives get a reasoned reply, once he approves it.
- **Failing checks** get the same treatment as bot findings.
- **A maintainer's question** goes to Jackson as a `maintainer_question` ask.
- **A maintainer's nod** on an offered fix: with text approved in advance, rebase, re-test, open the PR, then tell him. Without it, bring him the ready PR and wait. Speed matters: be ready to open a PR as soon as a maintainer agrees.
- **Someone else opens a PR** for our item: don't open ours. Compare their approach with ours, offer a 👍 or a useful fact if Jackson agrees, and move the item to `held` with a `covering` link.
- **Rebases:** force-push with lease only after re-checking the remote head, and only for a purely mechanical change. Additions go on as new commits. GitHub stacks can't cross forks, so wait for an outsider's base PR to merge rather than stacking on it.
- **Third-party noise** (cross-links, +1s from others): one line in your report, or nothing.

## 7. Close out [closed]

- Set the outcome: `merged`, `fixed_by_maintainer_pr`, `taken_over`, `covered_by_contributor`, `fixed_upstream`, `closed_by_upstream`, `not_a_giveback`, `parked` or `dropped`. Stop watching a closed item.
- **J5 follow-up.** When the fix is upstream, J5 drops its own copy at its next upstream advance. That means retiring a FORK.md case, or a D-number in `docs/j5/product/upstream.md`, in the J5 Code source. Record it as `followUpState: owed` with a `followUpNote` naming what to retire. Tell the J5 advance tracker issue on `Jacksondr5/j5code`. That work happens in J5 Code, not here.
