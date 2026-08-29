# Punk Poster Layout Audit Handoff

Date: 2026-08-29

## Objective

Continue the audit and refinement of `plugins/punk-poster-layout/skills/punk-poster-layout/` against the two source snapshots in `D:\WorkSpace\newskill`. The user requires a child-agent-driven workflow: assign each distinct task to a fresh agent, then have the primary agent review the result before starting the next task.

Do not commit or push unless the user explicitly asks.

## Suggested skills

- `skill-creator`: validate the skill structure, trigger description, progressive disclosure, and behavioral quality.
- `dispatching-parallel-agents`: use only as workflow guidance; the user wants fresh child agents, but the tasks here have intentionally been run sequentially so the primary agent can review between them.
- `handoff`: use again only if another continuation note is requested.

## Sources and governing files

- Workspace guidance: `D:\WorkSpace\AGENTS.md`
- Project guidance: `D:\WorkSpace\ShanireZskills\AGENTS.md`
- Source snapshots:
  - `D:\WorkSpace\newskill\AI 做海报、HTML构图手册：16 种控制焦点与动势的海报构图——上册（附样图和提示词）.mhtml`
  - `D:\WorkSpace\newskill\AI 做海报、HTML 构图手册：16 种控制网格、图文关系与信息密度的海报构图——下册.mhtml`
- Skill entry: `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\SKILL.md`
- Skill references: `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\references\`

## Work completed

### 1. Source-completeness audit

A fresh read-only child agent compared the packaged skill with both source snapshots. It confirmed that all 32 compositions, 32 image-generation prompts, 32 HTML/CSS prompts, the six structural variables, templates, Grid explanation, and review rules are present.

It found four issues:

- The two exact source task-to-composition tables were missing: six mappings from the upper article and nine mappings from the lower article.
- The source's controlled-break rule had been strengthened incorrectly into an absolute “break once” rule.
- Two locally synthesized combination examples were not labelled as skill-added synthesis.
- The Gestalt sentence had been strengthened from “influences” to “determines” and added a grouping principle not present in the source.

The primary agent independently extracted and visually checked the two selector-table images from the MHTML snapshots before accepting these findings.

### 2. Behavioral forward audit

A separate fresh read-only child agent simulated five requests: a sparse perfume poster prompt, a dense conference HTML poster, an experimental music festival poster, a WeChat article cover, and an ordinary SaaS hero.

Accepted findings:

- The frontmatter trigger was too broad and could capture ordinary Web/UI tasks.
- The boundary with `punk-cover` and ownership of visual style were unclear.
- Recommendation, prompt, generated poster, HTML/CSS, and review lacked explicit action/completion contracts.
- Several responsive rules preserved geometry at the cost of readability and accessibility.
- Required factual inputs and stop gates needed clarification without turning every missing structural field into a hard stop.
- Preflight/spec lint had to be separated from post-render artifact review.
- Exact-copy image-generation requests needed a no-text/overlay fallback.

### 3. Implemented source and behavioral fixes

A third fresh child agent implemented the accepted fixes. The primary agent reviewed the actual diff line by line.

Current modified files:

- `D:\WorkSpace\ShanireZskills\CHANGELOG.md`
- `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\SKILL.md`
- `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\references\composition-selector.md`
- `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\references\focus-and-motion.md`
- `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\references\grid-and-density.md`
- `D:\WorkSpace\ShanireZskills\plugins\punk-poster-layout\skills\punk-poster-layout\references\prompting-and-review.md`

The diff restores the two source selector tables, narrows the trigger, gives cover workflows priority, makes the skill structure-only, adds deliverable/action boundaries, protects factual copy, marks numeric values as adjustable starting points, relaxes the controlled-break rule accurately, separates preflight from artifact review, adds accurate-text fallback, and replaces rigid responsive geometry with semantic hierarchy and accessibility constraints.

The implementation agent reported these passing checks:

- `PYTHONUTF8=1` skill-creator `quick_validate.py`: `Skill is valid!`
- `git diff --check`
- `npx skills add . --list`: 20 skills found
- `claude plugin validate . --strict`

The primary agent separately confirmed that the working tree contained only the six files above before the next task began.

### 4. Packaging, distribution, and licensing audit

A fourth fresh read-only child agent checked the current 20-skill marketplace, manifests, attribution, independent-install packaging, references, and validation coverage.

Confirmed good:

- The plugin, Codex marketplace, Claude marketplace, and README each expose 20 unique skills with matching name sets.
- All four `SKILL.md` reference links resolve.
- Both detailed catalogs contain continuous and unique compositions 01–32, with exactly 32 image prompts and 32 HTML/CSS prompts.
- Both plugin-root and skill-root `LICENSE` files are byte-identical.
- Both plugin-root and skill-root `NOTICE` files are byte-identical, so independent skill installation retains the source URLs and attribution.
- All 67 JSON files and all 6 YAML files parsed.
- `quick_validate.py`, `npx skills add . --list`, `claude plugin validate . --strict`, and `git diff --check` passed.
- No unrelated K12 or other skill files were modified.

Findings still requiring action or a decision:

1. External discovery text is broader than the tightened `SKILL.md` trigger. Synchronize the three `punk-poster-layout` plugin manifests, the root Claude marketplace description, README row, and `agents/openai.yaml` so they say this is for standalone posters/flyers/key visuals/single-canvas editorial sections when explicit structural composition control is requested. Ordinary pages/UI/generic heroes and covers without explicit composition needs must not be default routes.
2. Review currently says “do not auto-fix” absolutely. Change it to “review reports evidence by default; modify only when the user explicitly asks for a fix.” Apply this consistently in the `SKILL.md` action matrix/workflow and artifact-review reference.
3. Attribution should distinguish the original articles from the Agent Skill adaptation and packaging. Preserve AdrianPunk as the source author, while naming ShanireZskills contributors as adaptation/packaging contributors in the two identical plugin/skill notices, the root notice, README, and preferably the three manifest author strings.
4. The repository does not contain auditable evidence that the two articles may be adapted and publicly redistributed under GPL. The existing 2026-08-29 GPL confirmation explicitly covers `punk-cover` and `punk-avatar`, not these two articles. This cannot be repaired by wording: obtain and retain written permission covering article adaptation, public redistribution, and the exact license version before public release.
5. `GPL-3.0` is an ambiguous/deprecated SPDX identifier. Do not silently change it to `GPL-3.0-or-later` or `GPL-3.0-only`: first confirm the actual grant with the rights holder and align the repository-wide Punk licensing convention.

## Interrupted task

Fresh child agent `/root/packaging_contract_fixes` was started to implement findings 1–3 plus the conditional review fix, but was interrupted at the user's request before it edited any file. After interruption, `git status` and `git diff --stat` were unchanged: only the six files listed in section 3 remain modified.

The approved implementation scope for the next fresh agent is:

- Synchronize discovery descriptions in:
  - `plugins/punk-poster-layout/plugin.json`
  - `plugins/punk-poster-layout/.codex-plugin/plugin.json`
  - `plugins/punk-poster-layout/.claude-plugin/plugin.json`
  - `.claude-plugin/marketplace.json`
  - `README.md`
  - `plugins/punk-poster-layout/skills/punk-poster-layout/agents/openai.yaml`
- Make review non-mutating by default but allow an explicit user-requested fix in `SKILL.md` and `references/prompting-and-review.md`.
- Clarify source authorship versus adaptation/packaging in the root, plugin, and skill notices and related metadata. Keep the plugin and skill notices byte-identical.
- Fold the metadata/attribution improvement into the existing current CHANGELOG bullet rather than adding a duplicate bullet.
- Do not claim that the GPL authorization gap is solved; do not change the license version identifier without rights-holder/maintainer confirmation.

## Recommended continuation sequence

1. Re-check `git status --short --branch` and `git diff --check`.
2. Spawn a new implementation child agent for the approved scope above; do not resume the interrupted agent as a substitute for the user's “one fresh agent per task” workflow.
3. Primary agent reviews every changed file and rejects any invented licensing claim.
4. Spawn another fresh read-only regression agent against the updated skill. Re-run the five behavioral requests and verify that ordinary SaaS heroes and unqualified platform covers no longer trigger the skill, while explicit standalone-poster work still does.
5. Primary agent runs the full project validation suite from `D:\WorkSpace\ShanireZskills`:
   - parse all JSON and YAML;
   - `PYTHONUTF8=1` skill-creator `quick_validate.py` for `punk-poster-layout`;
   - verify continuous unique 01–32, 32 image prompts, and 32 HTML/CSS prompts;
   - verify plugin/skill NOTICE and LICENSE hashes;
   - `npx skills add . --list`;
   - `claude plugin validate . --strict`;
   - `git diff --check`;
   - inspect `git status`, `git diff --stat`, and the final focused diff.
6. Report the technical audit as complete only if those checks pass. Separately report the article-licensing evidence as unresolved and do not describe the package as publication-ready until authorization and the exact GPL version are confirmed.

## Current repository state at handoff

- Branch: `main`, tracking `origin/main`.
- No commit or push was made in this audit.
- `handoff/` did not exist before this document was added.
- Before adding this document, `git diff --check` passed.
