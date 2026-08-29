# AGENTS.md

> Collective workspace rules live in [`../AGENTS.md`](../AGENTS.md). This file is the project-specific guideline for **ShanireZskills**, read by all agent runtimes (Claude Code, Codex, Gemini, Cursor, etc.).

## What this repo is

A public Agent Skills marketplace containing 20 independently installable plugins. GitHub is the primary repository; [CNB](https://cnb.cool/Round1/ShanireZskills) is the mirror synced by [`../sync-github-cnb.ps1`](../sync-github-cnb.ps1).

The market machine identifier is `shanirezskills`; the user-facing name is `ShanireZskills`. Each plugin contains exactly one same-named Agent Skill:

```text
plugins/
├── shanirez-style/             # first-party, GPL-3.0
├── k12-lesson-plan-creation/   # vendored, Apache-2.0
├── k12-lesson-differentiation/ # vendored, Apache-2.0
├── k12-lesson-prep/            # vendored, Apache-2.0
├── k12-check-for-understanding/ # vendored, Apache-2.0; math only
├── punk-cover/                 # vendored, GPL-3.0; 25 cover style atoms
├── punk-avatar/                # vendored, GPL-3.0; 6 avatar style atoms
├── punk-poster-layout/         # article-derived, GPL-3.0; 32 composition systems
└── <12 Emil Kowalski skills>/  # vendored, MIT
    └── skills/<same-name>/

.claude-plugins/                # Claude-only source mirrors for 3 explicit-invocation skills
├── pick-ui-library/
├── prototype/
└── review-animations/
```

Distribution adapters live at:

- `plugins/*/plugin.json` — Agent Plugins v1 manifests.
- `plugins/*/.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json` — Codex/ChatGPT.
- `plugins/*/.claude-plugin/plugin.json`, `.claude-plugins/*/.claude-plugin/plugin.json`, and `.claude-plugin/marketplace.json` — Claude Code and compatible marketplace clients.
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

The four skill trees under `plugins/k12-{lesson-plan-creation,lesson-differentiation,lesson-prep,check-for-understanding}/skills/` are copied verbatim from `anthropics/k12-teacher-skills` commit `281eb8d41fe2837d911541c9bbb870b58add804c`.

- Preserve each directory's Apache-2.0 `LICENSE`, SPDX header, and reference `NOTICE` files.
- Preserve the root `NOTICE` attribution when redistributing any K-12 skill.
- Do not silently edit vendored files. If a local change is necessary, add the prominent modification notice required by Apache-2.0 and record it in `CHANGELOG.md`.
- The optional upstream `.mcp.json` is intentionally not vendored; skills that can use the Learning Commons connector document their no-connector fallback.
- `.gitattributes` disables whitespace diagnostics only for these four directories so `git diff --check` can pass without changing upstream blobs.

## Vendored Emil Kowalski skills

The 12 skill directories imported from `emilkowalski/skills` are pinned to commit `d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7` and remain MIT-licensed. Preserve the root `NOTICE` attribution and the MIT `LICENSE` copies at both plugin and skill roots.

- Nine primary skill trees are copied verbatim. Do not silently edit their upstream files; record any intentional adaptation in `NOTICE` and `CHANGELOG.md`.
- `pick-ui-library`, `prototype`, and `review-animations` use `agents/openai.yaml` with `policy.allow_implicit_invocation: false` in their primary plugin trees because that is Codex's invocation-policy contract. Their Markdown bodies remain upstream-identical; only the invocation frontmatter is translated, and `review-animations` adds an explicit-only sentence to its description.
- `.claude-plugins/{pick-ui-library,prototype,review-animations}/` are exact upstream source mirrors used only by `.claude-plugin/marketplace.json`. They retain Claude's `disable-model-invocation: true`; keep them byte-identical to the pinned upstream skill trees.
- The Claude mirror directories are packaging adapters, not extra marketplace products. The public marketplace contains 20 names, not 23.

## Vendored Punk skills

`plugins/punk-cover/skills/punk-cover/`, `plugins/punk-avatar/skills/punk-avatar/`, and their required `styles/` subsets are copied from `adrianpunk/Punk-Skill` commit `50ea29b65b98788f9ed1df62818dbe530855bfb3` and distributed under GPL-3.0.

- Preserve the upstream `SKILL.md`, `agents/openai.yaml`, references, and selected style atoms verbatim. Record any intentional adaptation in `NOTICE` and `CHANGELOG.md`.
- The pinned upstream commit has no license file. GPL-3.0 was confirmed to the maintainer on 2026-08-29; canonical GPLv3 copies at each plugin and skill root are packaging additions, not upstream blobs.
- `punk-cover` carries only its 25 cover-capable style atoms; `punk-avatar` carries only its six avatar-capable style atoms. Their `../../styles/{style-id}` runtime paths depend on those plugin-root `styles/` directories.
- Upstream screenshots and repository-level validation scripts are not runtime dependencies and are intentionally not vendored.

## Punk poster-layout skill

`plugins/punk-poster-layout/skills/punk-poster-layout/` is an Agent Skill adaptation of two Punk Space articles by AdrianPunk, not a verbatim tree from the pinned `Punk-Skill` commit. The source URLs and attribution are recorded in `NOTICE`, and ShanireZskills distributes the adaptation under GPL-3.0.

- Preserve all 32 named composition systems, their image-prompt and HTML/CSS expressions, and the review criteria when reorganizing the references.
- Keep the skill's role structural: it selects and encodes focal flow, hierarchy, grids, image-text relationships, and density. It does not own the visual style atoms used by `punk-cover`.
- The original MHTML snapshots are source material outside the plugin and are not runtime assets. Do not add incomplete web archives or lazy-loaded remote images to the plugin package.

## Format constraints

- `plugins/shanirez-style/skills/shanirez-style/SKILL.md` keeps only `name` and `description` in frontmatter. Its name must equal the directory name, and it stays under 500 lines.
- The vendored K-12 skills retain their upstream `license` frontmatter field and Apache notices.
- Vendored Emil Kowalski skills retain their upstream frontmatter except for the three documented cross-client invocation-policy translations above.
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

### Documentation system

Maintain durable documentation as an OKF knowledge bundle. See `docs/agents/documentation.md` and `docs/agents/index.md`.
