# AGENTS.md

> Collective workspace rules live in [`../AGENTS.md`](../AGENTS.md). This file is the project-specific guideline for **Aptinery**, read by all agent runtimes (Claude Code, Codex, Gemini, Cursor, etc.).

## What this repo is

Aptinery — A curated foundry for Agent Skills. It is a public marketplace containing 20 independently installable plugins, combining first-party skills with curated, cross-client adaptations of non-standard upstream skills. [GitHub `ShanireZ/Aptinery`](https://github.com/ShanireZ/Aptinery) is authoritative; [CNB `Round1/Aptinery`](https://cnb.cool/Round1/Aptinery) is its mirror, synced by [`../sync-github-cnb.ps1`](../sync-github-cnb.ps1). ★ 名字已于 2026-08-31 收敛：本地目录、两个远端仓、两个本地 remote URL 全是 `Aptinery`，不再靠 GitHub 的改名重定向工作。

The marketplace machine identifier is `aptinery`; the user-facing name is `Aptinery`. Each plugin contains exactly one same-named Agent Skill:

布局与许可归属（权威是 `.claude-plugin/marketplace.json` 与各 `plugin.json`，用 `ls plugins/` 现查，不在此维护清单）：

- `plugins/shanirez-style/` — 一方，GPL-3.0。
- `plugins/k12-*/`（4 个）— vendored，Apache-2.0。
- `plugins/punk-cover/`、`plugins/punk-avatar/` — vendored，**上游未声明许可**。
- `plugins/punk-poster-layout/` — 文章改编，GPL-3.0。
- 其余 12 个 Emil Kowalski 技能 — vendored，MIT。
- `.claude-plugins/{pick-ui-library,prototype,review-animations}/` — Claude 专用源镜像，是打包适配器**不是额外产品**：市场只有 20 个名字，不是 23。

Distribution adapters live at:

- `plugins/*/plugin.json` — Agent Plugins v1 manifests.
- `plugins/*/.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json` — Codex/ChatGPT.
- `plugins/*/.claude-plugin/plugin.json`, `.claude-plugins/*/.claude-plugin/plugin.json`, and `.claude-plugin/marketplace.json` — Claude Code and compatible marketplace clients.
- `plugins/*/skills/` — skills CLI and direct Agent Skills discovery.

Keep `0.2.0`-style versions synchronized across all plugin and marketplace manifests.

## First-party style source of truth

`plugins/shanirez-style/skills/shanirez-style/SKILL.md` 里的每一条统计都是对 [`../OJCode`](../OJCode) 语料实测出来的——**不重新跑一遍计数，就不要新增、削弱或强化任何一条断言**。复算方法与四个易错点见 [`docs/style-corpus-method.md`](docs/style-corpus-method.md)。

## Vendored K-12 skills

The four skill trees under `plugins/k12-{lesson-plan-creation,lesson-differentiation,lesson-prep,check-for-understanding}/skills/` are copied verbatim from `anthropics/k12-teacher-skills` commit `281eb8d41fe2837d911541c9bbb870b58add804c`.

- Preserve each directory's Apache-2.0 `LICENSE`, SPDX header, and reference `NOTICE` files.
- Preserve the root `NOTICE` attribution when redistributing any K-12 skill.
- Do not silently edit vendored files. If a local change is necessary, add the prominent modification notice required by Apache-2.0 and record it in `NOTICE`.
- The optional upstream `.mcp.json` is intentionally not vendored; skills that can use the Learning Commons connector document their no-connector fallback.
- `.gitattributes` disables whitespace diagnostics only for these four directories so `git diff --check` can pass without changing upstream blobs.

## Vendored Emil Kowalski skills

The 12 skill directories imported from `emilkowalski/skills` are pinned to commit `d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7` and remain MIT-licensed. Preserve the root `NOTICE` attribution and the MIT `LICENSE` copies at both plugin and skill roots.

- Nine primary skill trees are verbatim. Do not silently edit upstream files; record any adaptation in `NOTICE`.
- `pick-ui-library`/`prototype`/`review-animations` carry `agents/openai.yaml` with `policy.allow_implicit_invocation: false` (Codex's invocation contract). Markdown bodies stay upstream-identical — only the invocation frontmatter is translated, plus one explicit-only sentence in `review-animations`'s description.
- `.claude-plugins/{pick-ui-library,prototype,review-animations}/` are exact upstream source mirrors used only by `.claude-plugin/marketplace.json`. They retain Claude's `disable-model-invocation: true`; keep them byte-identical to the pinned upstream skill trees.

## Vendored Punk skills without a declared upstream license

`plugins/punk-cover/skills/punk-cover/`, `plugins/punk-avatar/skills/punk-avatar/`, and their required `styles/` subsets are copied from `adrianpunk/Punk-Skill` commit `50ea29b65b98788f9ed1df62818dbe530855bfb3`. That pinned upstream commit contains no license declaration, so Aptinery does not assign, infer, or supplement one; redistribution permission is not established.

- Preserve the upstream `SKILL.md`, `agents/openai.yaml`, references, and selected style atoms verbatim. Record any intentional adaptation in `NOTICE`.
- Do not restore the removed GPLv3 copies or claims from Git history, archives, caches, or prior records. A future license declaration requires explicit, independently verifiable evidence from the rights holder.
- `punk-cover` carries only its 25 cover-capable style atoms; `punk-avatar` carries only its six avatar-capable style atoms. Their `../../styles/{style-id}` runtime paths depend on those plugin-root `styles/` directories.
- Upstream screenshots and repo-level validation scripts are not runtime deps; intentionally not vendored.

## Punk poster-layout skill

`plugins/punk-poster-layout/skills/punk-poster-layout/` is an Agent Skill adaptation of two Punk Space articles by AdrianPunk, not a verbatim tree from the pinned `Punk-Skill` commit. The source URLs and attribution are recorded in `NOTICE`, and Aptinery distributes the adaptation under GPL-3.0.

- Preserve all 32 named composition systems, their image-prompt and HTML/CSS expressions, and the review criteria when reorganizing the references.
- Keep the skill's role structural: it selects and encodes focal flow, hierarchy, grids, image-text relationships, and density. It does not own the visual style atoms used by `punk-cover`.
- The original MHTML snapshots are source material outside the plugin, not runtime assets. Do not add incomplete web archives or lazy-loaded remote images to the package.

## Format constraints

- `plugins/shanirez-style/skills/shanirez-style/SKILL.md` keeps only `name` and `description` in frontmatter. Its name must equal the directory name, and it stays under 500 lines.
- The vendored K-12 skills retain their upstream `license` frontmatter field and Apache notices.
- Vendored Emil Kowalski skills retain their upstream frontmatter except for the three documented cross-client invocation-policy translations above.
- `.gitattributes` pins text files to LF.
- The no-blank-lines/no-trailing-newline rule covers generated OJ `.cpp` and C++ samples only, not Markdown.
- Templates are code: compile changed C++ templates with `g++ -std=c++14 -O2 -Wall -m64 -static-libgcc` and run a hand-checked input.

## Validation

After distribution or skill changes, run:

```bash
npx skills add . --list
claude plugin validate . --strict
```

Also parse every JSON manifest, check vendored files against the recorded upstream commit when they were not intentionally modified, and finish with `git diff --check`.

## Research records

Do not create or restore `docs/research/` or standalone research/audit report files. Return ad hoc research in the conversation. When a verified licensing or provenance fact changes the distribution, record only the operative fact in the relevant source-of-truth file (`AGENTS.md`, `README.md`, or `NOTICE`); do not reconstruct removed reports from Git history, archives, caches, or prior records.

## Agent skills

- **Issue tracker：本仓 GitHub Issues。**
- triage 标签、domain 文档布局、OKF 文档系统沿用工作区约定：[`docs/agents/index.md`](docs/agents/index.md)。
- 按环节的守则、完成判据与技能对照见根 [`../Docs/dev_guide.md`](../Docs/dev_guide.md)，它每个会话自动加载。
