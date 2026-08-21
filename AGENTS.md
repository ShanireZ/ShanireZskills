# AGENTS.md

> Collective workspace rules live in [`../AGENTS.md`](../AGENTS.md). This file is the project-specific guideline for **ShanireZskills**, read by all agent runtimes (Claude Code, Codex, Gemini, Cursor, etc.).

## What this repo is

A public Agent Skills marketplace containing three independently installable plugins. GitHub is the primary repository; [CNB](https://cnb.cool/Round1/ShanireZskills) is the mirror synced by [`../sync-github-cnb.ps1`](../sync-github-cnb.ps1).

The market machine identifier is `shanirezskills`; the user-facing name is `ShanireZskills`. Each plugin contains exactly one same-named Agent Skill:

```text
plugins/
├── shanirez-style/             # first-party, GPL-3.0
│   └── skills/shanirez-style/
├── k12-lesson-planning/        # vendored, Apache-2.0
│   └── skills/k12-lesson-planning/
└── k12-lesson-differentiation/ # vendored, Apache-2.0
    └── skills/k12-lesson-differentiation/
```

Distribution adapters live at:

- `plugins/*/plugin.json` — Agent Plugins v1 manifests.
- `plugins/*/.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json` — Codex/ChatGPT.
- `plugins/*/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` — Claude Code and compatible marketplace clients.
- `plugins/*/skills/` — skills CLI and direct Agent Skills discovery.

Keep `0.1.0`-style versions synchronized across all plugin and marketplace manifests and `CHANGELOG.md`.

## First-party style source of truth

Every statistic in `plugins/shanirez-style/skills/shanirez-style/SKILL.md` is measured against the [`../OJCode`](../OJCode) corpus — do not add, soften, or strengthen a claim without re-running the count. Method:

```bash
cd D:/Workspace/OJCode
git ls-files '*.cpp' > /tmp/all.txt
git log --diff-filter=A --since="<2 years ago>" --name-only --format="" -- '*.cpp' \
  | grep -v '^$' | sort -u > /tmp/added.txt
while IFS= read -r f; do [ -f "$f" ] && printf '%s\n' "$f"; done \
  < /tmp/added.txt > /tmp/recent.txt
xargs -a /tmp/recent.txt -d '\n' grep -lIE '<pattern>' | wc -l
```

- `--diff-filter=A` alone is not the recent set; intersect it with files that still exist.
- Where a habit shifted, the last two years win.
- State whether a percentage is whole-corpus or recent.
- Use `xargs -a <list> -d '\n'`; filenames contain spaces.
- Grep before asserting a name or the absence of a form.

## Vendored K-12 skills

`plugins/k12-lesson-planning/skills/k12-lesson-planning/` and `plugins/k12-lesson-differentiation/skills/k12-lesson-differentiation/` are copied verbatim from `anthropics/k12-teacher-skills` commit `6fc400329540e068516bd34aa78120d89e5e4e8b`.

- Preserve each directory's Apache-2.0 `LICENSE`, SPDX header, and reference `NOTICE` files.
- Preserve the root `NOTICE` attribution when redistributing either skill.
- Do not silently edit vendored files. If a local change is necessary, add the prominent modification notice required by Apache-2.0 and record it in `CHANGELOG.md`.
- The optional upstream `.mcp.json` is intentionally not vendored; both skills document their no-connector fallback.
- `.gitattributes` disables whitespace diagnostics only for these two directories so `git diff --check` can pass without changing upstream blobs.

## Format constraints

- `plugins/shanirez-style/skills/shanirez-style/SKILL.md` keeps only `name` and `description` in frontmatter. Its name must equal the directory name, and it stays under 500 lines.
- The vendored K-12 skills retain their upstream `license` frontmatter field and Apache notices.
- `.gitattributes` pins text files to LF.
- The no-blank-lines/no-trailing-newline rule applies to generated OJ `.cpp` and C++ samples, not Markdown generally.
- Templates are code: compile changed C++ templates with `g++ -std=c++14 -O2 -Wall -m64 -static-libgcc` and run a hand-checked input.

## Validation

After distribution or skill changes, run:

```bash
npx skills add . --list
claude plugin validate . --strict
```

Also parse every JSON manifest, check vendored files against the recorded upstream commit when they were not intentionally modified, and finish with `git diff --check`.

## Agent skills

### Issue tracker

Issues and specs are tracked in this repository's GitHub Issues. See `docs/agents/issue-tracker.md`.

### Triage labels

Use the five canonical triage labels. See `docs/agents/triage-labels.md`.

### Domain docs

This target uses a single-context domain-doc layout. See `docs/agents/domain.md`.

### Related engineering skills

See `docs/agents/skill-workflows.md` for recommendations on when to use the installed engineering skills and how their workflows compose.
