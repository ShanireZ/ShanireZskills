# AGENTS.md

> Collective workspace rules live in [`../AGENTS.md`](../AGENTS.md). This file is the project-specific guideline for **Shanirez-Style**, read by all agent runtimes (Claude Code, Codex, Gemini, Cursor, etc.).

## What this repo is

A single Agent Skill — **not** an application. It packages ShanireZ's competitive-programming C++ style so that agents generating or editing OJ solutions produce code indistinguishable from the author's own. Private repo; GitHub is the primary, [CNB](https://cnb.cool/Round1/Shanirez-Style) is the mirror (synced by [`../sync-github-cnb.ps1`](../sync-github-cnb.ps1)).

```
skills/shanirez-style/
├── SKILL.md                  # the style fingerprint (frontmatter + body)
└── references/templates.md   # 13 algorithm templates in house style
```

The nested `skills/<name>/` layout is deliberate: it matches the [skills CLI](https://github.com/vercel-labs/skills) convention (`npx skills add ShanireZ/Shanirez-Style`), which installs into `~/.agents/skills/` where **both Codex and Claude Code** discover it.

## Source of truth for style claims

Every statistic in `SKILL.md` is measured against the [`../OJCode`](../OJCode) corpus — do not add, soften, or strengthen a claim without re-running the count. Method:

```bash
cd D:/Workspace/OJCode
git ls-files '*.cpp' > /tmp/all.txt                                        # all 3208 files
git log --diff-filter=A --since="<2 years ago>" --name-only --format="" -- '*.cpp' \
  | grep -v '^$' | sort -u > /tmp/added.txt                                # 948 added
while IFS= read -r f; do [ -f "$f" ] && printf '%s\n' "$f"; done \
  < /tmp/added.txt > /tmp/recent.txt                                       # 940 still on disk
xargs -a /tmp/recent.txt -d '\n' grep -lIE '<pattern>' | wc -l
```

- **`--diff-filter=A` alone is not the recent set.** Files added and later deleted or moved stay in that list (the removed `CCFCSP/` folder contributes 8), so intersect it with what still exists before counting — otherwise every denominator is wrong.
- **Where a habit shifted, the last two years win.** Several conventions flipped (`sort(a + 1, a + n + 1)` overtook `a + 1 + n`; `1e9` overtook `0x3f3f3f3f`; 链式前向星 went to zero). The all-time average is the wrong answer for "the author's current hand".
- **A whole-corpus percentage is not a recent percentage.** `read()` is ~9% all-time but 1.4% recent; `// TAG:` is 7.6% all-time but 1.8% recent. Say which set a number comes from.
- Use `xargs -a <list> -d '\n'`, not a per-file loop — a shell loop over 3200 files takes >10 minutes.
- Filenames contain spaces (`SP UVA AT/`, `B3609 Tarjan.cpp`); always pass `-d '\n'`.
- Beware of `queue<..> que`-style guesses: the corpus says `q`. Grep before asserting a name, including for the *absence* of a form (`!q.empty()`, `dx`/`dy`, lambdas are all zero).

## Format constraints (don't break these)

`SKILL.md` must stay loadable by both runtimes:

- Frontmatter carries **only** `name` and `description`. The Agent Skills spec allows six keys (`name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`); anything else makes claude.ai uploads and the Skills API reject the file.
- `name` ≤ 64 chars, lowercase letters / digits / hyphens only, and must equal the containing directory name (case- and underscore-insensitive).
- `description` non-empty, ≤ 1024 chars, no XML tags. It is the only thing an agent sees before loading the body, so it must state both when to trigger **and** when not to.
- Keep `SKILL.md` under 500 lines; push detail into `references/`.

## Working in this repo

- No build system, no tests, no dependencies. Edits are prose.
- `.gitattributes` pins `* text=auto eol=lf` — the skill is consumed on Linux; don't commit CRLF.
- The skill's own content mandates that generated `.cpp` files have **no blank lines and no trailing newline**. That rule is about generated C++, not about the Markdown here. It *does* apply to the ```cpp samples in both Markdown files — a sample with a blank line teaches the wrong thing. Split the block instead.
- **Templates are code — verify them as code.** After changing one, paste it into a scratch `.cpp`, compile with the repo's own flags (`g++ -std=c++14 -O2 -Wall -m64 -static-libgcc`; MinGW 13.2 lives at `C:\mingw64\bin`), and run it on a hand-checked input. Every template here currently compiles warning-free and produces correct output.
