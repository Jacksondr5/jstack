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

Change a skill here, never in an installed copy: a refresh overwrites the copy.

### J5 Code

This repository is a J5 Code skill catalog: [`catalog.yaml`](catalog.yaml)
sorts the skills into groups. In J5 Code, open **Settings → Skills**, set the
catalog source to `https://github.com/Jacksondr5/jstack.git` or to the absolute
path of a local clone, pick the groups you want and apply. J5 Code links the
skills into `~/.agents/skills/` and `~/.claude/skills/` rather than copying
them, so a local clone's edits reach agents right away. **Update** pulls a
GitHub source. `take-the-wheel` isn't in the catalog; install it with the
`skills` CLI.

J5 Code won't replace a skill folder the `skills` CLI installed. Remove those
copies first, for example `npx skills remove -g -s council`, then apply.

## Rules for every agent

[`global/AGENTS.md`](global/AGENTS.md) holds my preferences for every agent, on
every machine. Each machine keeps its own rules (hosts, ports, local services)
in `~/.config/jstack/AGENTS.local.md`. [`global/install.sh`](global/install.sh)
joins the two into `~/.codex/AGENTS.md`, which Codex reads and Claude Code
includes from `~/.claude/CLAUDE.md` with `@~/.codex/AGENTS.md`. Rerun it after
changing either file. It keeps a hand-written `~/.codex/AGENTS.md` as a backup
before replacing it.

This repository is public, so `global/AGENTS.md` holds only rules that make
sense to anyone. Anything about my machines, accounts or work goes in the local
file.

## Credits

`council` and `take-the-wheel` are adapted from Theo Browne's
[tstack](https://github.com/t3dotgg/tstack). `take-the-wheel` is based on his
`full-send` skill. His skills are MIT licensed. Each skill adapted from his
includes his license in `LICENSE.txt`, and names the original in the `metadata`
field of its `SKILL.md`.

## License

[MIT](LICENSE). The adapted skills also carry Theo's copyright in their
`LICENSE.txt`.
