# Changelog

## Unreleased

- **Breaking:** replace the published `k12-lesson-planning` plugin and skill name with upstream's `k12-lesson-plan-creation`; no compatibility alias is retained.
- Refresh all existing K-12 skill files and attribution from `anthropics/k12-teacher-skills` commit `281eb8d41fe2837d911541c9bbb870b58add804c`.
- Expand the marketplace from 18 to 20 independently installable skills by adding `k12-lesson-prep` and the math-only `k12-check-for-understanding` under Apache-2.0.
- Expand the marketplace from 17 to 18 independently installable skills by adding `punk-poster-layout`, a 32-system poster-composition skill derived from AdrianPunk's two Punk Space articles on focal flow, grids, image-text relationships, and information density.
- Package the new skill for Agent Plugins, Codex/ChatGPT, Claude Code, GitHub Copilot CLI, and the skills CLI, with separate progressive-disclosure references for selection, detailed composition rules, prompting, HTML/CSS, and review.
- Tighten `punk-poster-layout` with the source articles' complete task-to-composition selectors, explicit scope and action boundaries, controlled-break semantics, accurate-text fallback, evidence-based responsive/accessibility review rules, synchronized narrow-scope discovery metadata, and separate attribution for AdrianPunk's source articles versus ShanireZskills adaptation and packaging.
- Expand the marketplace from 15 to 17 independently installable skills by vendoring `punk-cover` and `punk-avatar` from `adrianpunk/Punk-Skill` commit `50ea29b65b98788f9ed1df62818dbe530855bfb3` under GPL-3.0.
- Package each Punk skill independently for Agent Plugins, Codex/ChatGPT, Claude Code, GitHub Copilot CLI, and the skills CLI while retaining only the style atoms it can select.
- Add canonical GPLv3 license copies and a transparent licensing record because the pinned upstream commit does not contain a license file.
- Expand the marketplace from 3 to 15 independently installable skills by vendoring all 12 MIT-licensed skills from `emilkowalski/skills` commit `d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7`.
- Add Agent Plugins, Codex/ChatGPT, Claude Code, GitHub Copilot CLI, and skills CLI packaging for every new skill.
- Preserve Claude's explicit-only invocation contract for `pick-ui-library`, `prototype`, and `review-animations` through exact Claude marketplace mirrors while mapping the same policy to `agents/openai.yaml` for Codex.
- Split the former all-in-one marketplace package into three independently installable, single-skill plugins.
- Rename the GitHub repository, CNB mirror, and local project directory to `ShanireZskills`.
- Remove the `v0.1.0` Git tag while keeping `0.1.0` as the current marketplace package version.

## 0.1.0 — 2026-08-13

- Publish the repository as the `ShanireZskills` marketplace.
- Package the skills for Agent Plugins, Codex/ChatGPT, Claude Code, GitHub Copilot CLI, and the skills CLI.
- Add `k12-lesson-planning` and `k12-lesson-differentiation` from Anthropic's Apache-2.0-licensed `k12-teacher-skills` repository at commit `6fc400329540e068516bd34aa78120d89e5e4e8b`.
- License original repository content under GNU GPL v3.0 while preserving the imported skills' Apache-2.0 terms and notices.
