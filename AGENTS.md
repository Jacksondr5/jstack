# jstack

Agent skills for Claude Code and Codex. Each skill is a folder under `skills/`
with a `SKILL.md`.

## Keep `catalog.yaml` in step with `skills/`

J5 Code installs these skills from [`catalog.yaml`](catalog.yaml). Whenever
you add, rename or remove a skill, update the catalog in the same change.

- Every folder under `skills/` is listed in exactly one group. A skill that is
  missing from the catalog can't be installed, and a skill listed twice is
  rejected.
- A name in `skills:` is the skill's folder name, and that folder must contain
  `SKILL.md`. One missing `SKILL.md` makes every Apply that selects its group
  fail.
- Group and skill names are lowercase letters, digits and hyphens, starting
  with a letter or digit.
- Each group needs a `description` and at least one skill. `depends:` lists
  other groups that are installed along with it. The only top-level key is
  `groups`, and unknown keys are rejected.
- Renaming a group drops it from the selection of everyone who picked it, and
  the next Apply unlinks its skills. Prefer adding a group to renaming one.
- When you change the skills, update the table in `README.md` too.

Check that every skill folder is listed exactly once:

```bash
diff <(ls skills | sort) <(grep -oE 'skills: \[[^]]*\]' catalog.yaml | sed -E 's/skills: \[|\]//g' | tr ',' '\n' | tr -d ' ' | sort)
```

No output means they match.
