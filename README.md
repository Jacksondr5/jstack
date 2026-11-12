<img src="assets/favicon.svg" width="64" alt="jstack logo">

# jstack

The agent skills I use with Claude Code and Codex. Some use
[J5 Code](https://github.com/Jacksondr5/j5code) tools when they are available
and fall back to the harness's own tools when they are not.

## Skills

| Skill | Say | What it does |
| --- | --- | --- |
| [`council`](skills/council/SKILL.md) | "council" | Asks Claude Opus, GPT Sol, and Grok (when available) for a review, fixes what they agree on, and repeats until they are satisfied. Scores go in the PR description. |
| [`take-the-wheel`](skills/take-the-wheel/SKILL.md) | "take the wheel" | Hands the agent the work end to end, with nobody watching. Grants permission to use the test methods it needs without asking first. |
| [`t3code-giveback`](skills/t3code-giveback/SKILL.md) | (loads itself in a `pingdotgg/t3code` checkout) | Briefs agents giving J5 Code fixes back to T3 Code: Jackson's rules for posting upstream, the lifecycle of an item, and the giveback dashboard. |

## Install

Use the [`skills` CLI](https://github.com/vercel-labs/skills). It installs to
`~/.agents/skills/`, which Codex reads, and symlinks the skill into
`~/.claude/skills/` for Claude Code.

```bash
# From GitHub
npx skills add Jacksondr5/jstack -g -a claude-code codex -s council

# From a local clone
npx skills add ~/repos/jacksondr5/jstack -g -a claude-code codex -s council
```

Use `-s '*'` to install every skill. The CLI copies the skill, so edits to
this repository do not reach the installed copy. Run the same `add` command
again to pick them up. `npx skills update -g` refreshes GitHub installs, but it
skips local-path installs.

## Credits

`council` and `take-the-wheel` are adapted from Theo Browne's
[tstack](https://github.com/t3dotgg/tstack). `take-the-wheel` is based on his
`full-send` skill. His skills are MIT licensed. Each skill adapted from his
includes his license in `LICENSE.txt`, and names the original in the `metadata`
field of its `SKILL.md`.

## License

[MIT](LICENSE). The adapted skills also carry Theo's copyright in their
`LICENSE.txt`.
