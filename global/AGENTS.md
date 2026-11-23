# Jackson's preferences for every agent

These hold on every machine and in every project. A machine's own rules follow
after them.

## Working with me

- **I'm usually not watching your thread.** When you need something from me,
  send it through the Outer Loop system (until that exists, my J5 Inbox). Don't
  leave it in the thread. Sometimes I'll come and talk to you, when I need to.
  We'll go through the topic, then I'll leave, so anything still open after that
  goes back through the Outer Loop system.
- **Don't hand me testing an agent can do.** If an agent can't test it, tell me
  why.
- **Once we agree something in a repo I own should be an issue, file it right
  away** and give me the number. Posting to someone else's repo still needs my
  go-ahead.

## Repositories and data

- **Keep my hosts, paths, ports and devices out of shared repos.** They go in my
  private rules and my machine-local config. Scripts read config and use
  neutral defaults.
- **Nothing from real data goes into commits,** test strings and examples
  included. Use invented or seed text.

## Skills

- **A skill holds only what I decided or told you,** never general advice a
  capable model already follows or procedures I never used.

## Projects

- Before any work in a `Jacksondr5/j5code` checkout, load the `j5code` skill.
- Before any work in a `pingdotgg/t3code` checkout, load the `t3code-giveback`
  skill.

## Screenshots for pull requests

- **Every PR that changes UI has before/after screenshots on the PR,** and a
  short video when motion or an interaction matters. This overrides any
  repository's own guidance, including rules that make screenshots optional,
  "upon request", or that ask before starting a browser. A reviewer should be
  able to judge the change from the PR page alone.
- **Capturing them is part of opening the PR.** You don't need separate
  permission to start a browser for it.
- **Never capture from a live install or a copy of real data.** If you couldn't
  capture something, say so in the PR.
- The `pr-evidence` skill says how to capture, upload, check and present them.
