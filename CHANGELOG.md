# Changelog

## Unreleased

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
