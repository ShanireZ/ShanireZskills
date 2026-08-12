# AGENTS.md

> Collective workspace rules live in [`../AGENTS.md`](../AGENTS.md). This file is the project-specific guideline for **Shanirez-Style**, read by all agent runtimes (Claude Code, Codex, Gemini, Cursor, etc.).

## What this repo is

A single Agent Skill — **not** an application. It packages ShanireZ's competitive-programming C++ style so that agents generating or editing OJ solutions produce code indistinguishable from the author's own. Private repo; GitHub is the primary, [CNB](https://cnb.cool/Round1/Shanirez-Style) is the mirror (synced by [`../sync-github-cnb.ps1`](../sync-github-cnb.ps1)).

```
skills/shanirez-style/
├── SKILL.md                  # the style fingerprint (frontmatter + body)
└── references/templates.md   # 12 algorithm templates in house style
```

The nested `skills/<name>/` layout is deliberate: it matches the [skills CLI](https://github.com/vercel-labs/skills) convention (`npx skills add ShanireZ/Shanirez-Style`), which installs into `~/.agents/skills/` where **both Codex and Claude Code** discover it.

## Source of truth for style claims

Every statistic in `SKILL.md` is measured against the [`../OJCode`](../OJCode) corpus — do not add, soften, or strengthen a claim without re-running the count. Method:

```bash
cd D:/Workspace/OJCode
git ls-files '*.cpp' > /tmp/all.txt                                        # all ~3200 files
git log --diff-filter=A --since="<2 years ago>" --name-only --format="" -- '*.cpp' \
  | grep -v '^$' | sort -u > /tmp/recent.txt                               # ~950 recent files
xargs -a /tmp/recent.txt -d '\n' grep -lIE '<pattern>' | wc -l
```

- **Where a habit shifted, the last two years win.** Several conventions flipped (`sort(a + 1, a + n + 1)` overtook `a + 1 + n`; `1e9` overtook `0x3f3f3f3f`; 链式前向星 went to zero). The all-time average is the wrong answer for "the author's current hand".
- Use `xargs -a <list> -d '\n'`, not a per-file loop — a shell loop over 3200 files takes >10 minutes.
- Filenames contain spaces (`SP UVA AT/`, `B3609 Tarjan.cpp`); always pass `-d '\n'`.

## Format constraints (don't break these)

`SKILL.md` must stay loadable by both runtimes:

- Frontmatter carries **only** `name` and `description`. The Agent Skills spec allows six keys (`name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`); anything else makes claude.ai uploads and the Skills API reject the file.
- `name` ≤ 64 chars, lowercase letters / digits / hyphens only, and must equal the containing directory name (case- and underscore-insensitive).
- `description` non-empty, ≤ 1024 chars, no XML tags. It is the only thing an agent sees before loading the body, so it must state both when to trigger **and** when not to.
- Keep `SKILL.md` under 500 lines; push detail into `references/`.

## Working in this repo

- No build system, no tests, no dependencies. Edits are prose.
- `.gitattributes` pins `* text=auto eol=lf` — the skill is consumed on Linux; don't commit CRLF.
- The skill's own content mandates that generated `.cpp` files have **no blank lines and no trailing newline**. That rule is about generated C++, not about the Markdown here.
