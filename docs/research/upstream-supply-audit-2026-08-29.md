---
type: research
title: Upstream supply audit — 2026-08-29
description: Checks the three recorded third-party skill suppliers against their pinned commits and current default branches.
tags: [agent-skills, marketplace, supply-chain, upstream-audit]
status: verified
follow_up: implemented
checked_at: "2026-08-29T10:03:12+08:00"
sources:
  - id: anthropic-compare
    resource: https://github.com/anthropics/k12-teacher-skills/compare/6fc400329540e068516bd34aa78120d89e5e4e8b...281eb8d41fe2837d911541c9bbb870b58add804c
    title: anthropics/k12-teacher-skills pin-to-main comparison
  - id: anthropic-head
    resource: https://github.com/anthropics/k12-teacher-skills/commit/281eb8d41fe2837d911541c9bbb870b58add804c
    title: anthropics/k12-teacher-skills current main head
  - id: emil-compare
    resource: https://api.github.com/repos/emilkowalski/skills/compare/d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7...d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7
    title: emilkowalski/skills pin-to-main comparison
  - id: emil-tree
    resource: https://github.com/emilkowalski/skills/tree/d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7/skills
    title: emilkowalski/skills current skill tree
  - id: punk-compare
    resource: https://api.github.com/repos/adrianpunk/Punk-Skill/compare/50ea29b65b98788f9ed1df62818dbe530855bfb3...50ea29b65b98788f9ed1df62818dbe530855bfb3
    title: adrianpunk/Punk-Skill pin-to-main comparison
  - id: punk-tree
    resource: https://github.com/adrianpunk/Punk-Skill/tree/50ea29b65b98788f9ed1df62818dbe530855bfb3/skills
    title: adrianpunk/Punk-Skill current skill tree
---

# Upstream Supply Audit

## Scope and result

This audit covers only the three repository suppliers recorded for formally vendored marketplace products. It excludes the first-party `shanirez-style` skill and the article-derived `punk-poster-layout`, which is published separately but is not a vendored tree from the recorded `Punk-Skill` pin.

One supplier has an actionable update: Anthropic's K-12 repository is one commit ahead and now carries four skills instead of two. Emil Kowalski's and AdrianPunk's default branches remain identical to the recorded pins.

| Supplier | Recorded pin | Current default-branch head | Ahead | Vendored impact | Recommendation |
|---|---|---|---:|---|---|
| `anthropics/k12-teacher-skills` | `6fc400329540e068516bd34aa78120d89e5e4e8b` | `281eb8d41fe2837d911541c9bbb870b58add804c` (`main`) | 1 | Existing vendored trees changed; one public skill was renamed; two skills were added upstream | Prepare a coordinated K-12 migration release; do not apply as a blind content refresh |
| `emilkowalski/skills` | `d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7` | same (`main`) | 0 | None | No update |
| `adrianpunk/Punk-Skill` | `50ea29b65b98788f9ed1df62818dbe530855bfb3` | same (`main`) | 0 | None | No update |

`Ahead` is GitHub Compare API's `ahead_by` value from the recorded pin to the repository's current default branch. Repository `updated_at` activity was not treated as a code update when the branch head was unchanged.

## Anthropic K-12

The `main` branch advanced by one commit, `281eb8d41fe2837d911541c9bbb870b58add804c`, dated 2026-08-28. GitHub reports `status: ahead`, `ahead_by: 1`, `behind_by: 0`, and 45 changed files; 33 changed paths are under `plugin/skills/`.[^anthropic-compare]

### Existing vendored content

- `k12-lesson-planning` was renamed upstream to `k12-lesson-plan-creation`. Compare reports 15 renamed paths and one added `LICENSE` path in the new tree. The content changes update the skill name and internal cross-references, expand the frontmatter license statement, and move SPDX attribution from leading HTML comments to trailing copyright lines. The renderer sources themselves are path renames without content changes.[^anthropic-compare]
- `k12-lesson-differentiation` changes eight vendored files: `SKILL.md`; `references/ela.md`, `learning-commons-kg.md`, `math.md`, `output.md`, `science.md`, and `social_studies.md`; and `scripts/render_all.sh`. The observed changes are the planning-to-plan-creation cross-reference, fuller Apache attribution, relocation of SPDX text, and a matching script comment; no instructional body was rewritten.[^anthropic-compare]

The rename was a marketplace compatibility decision, not merely a file move: ShanireZskills published `k12-lesson-planning`, while upstream declares `name: k12-lesson-plan-creation`.[^anthropic-compare]

### New upstream skills

The current upstream tree contains four skills instead of the two at the pin:[^anthropic-compare]

- `k12-check-for-understanding` — a new math formative-check skill with `SKILL.md`, `NOTICE`, four reference documents, and `LICENSE`. GitHub's similarity detection presents the identical license blob as a rename from the old planning tree, but the skill itself is new.
- `k12-lesson-prep` — a new teacher lesson-internalization skill with `SKILL.md` and `LICENSE`.
- `k12-lesson-differentiation` — existing, updated as above.
- `k12-lesson-plan-creation` — the renamed successor to `k12-lesson-planning`.

The remaining 12 changed paths are repository packaging, README, and evaluation-rubric changes rather than files currently vendored into ShanireZskills.[^anthropic-compare]

### Implemented migration

The maintainer selected an explicit breaking migration on 2026-08-29. It was applied as one coordinated market update:

1. `k12-lesson-differentiation` was refreshed from the new pin with its Apache-2.0 `LICENSE` and `NOTICE` obligations preserved.
2. `k12-check-for-understanding` and `k12-lesson-prep` were reviewed and packaged as independent products.
3. `k12-lesson-planning` was removed and replaced by `k12-lesson-plan-creation`; no compatibility alias or transition product remains.
4. Marketplace manifests, counts, attribution, and changelog entries were updated together.

## Emil Kowalski

The recorded pin is still the exact `main` head. GitHub reports `status: identical`, `ahead_by: 0`, `behind_by: 0`, zero commits, and zero changed files.[^emil-compare]

The current tree still contains exactly the same 12 skills: `animate-expo`, `animate`, `animation-vocabulary`, `apple-design`, `ask-sonner`, `emil-design-eng`, `find-animation-opportunities`, `improve-animations`, `pick-ui-library`, `prototype`, `review-animations`, and `write-swift`.[^emil-tree] There is no supplier update to import.

## AdrianPunk

The recorded pin is still the exact `main` head. GitHub reports `status: identical`, `ahead_by: 0`, `behind_by: 0`, zero commits, and zero changed files.[^punk-compare]

The current upstream repository tree still contains only `punk-avatar` and `punk-cover` as skills.[^punk-tree] There is no repository update to import. The locally published `punk-poster-layout` remains outside this comparison because it is article-derived rather than a vendored `Punk-Skill` tree.

[^anthropic-compare]: [Official GitHub comparison for `anthropics/k12-teacher-skills`](https://github.com/anthropics/k12-teacher-skills/compare/6fc400329540e068516bd34aa78120d89e5e4e8b...281eb8d41fe2837d911541c9bbb870b58add804c) and [current head commit](https://github.com/anthropics/k12-teacher-skills/commit/281eb8d41fe2837d911541c9bbb870b58add804c).
[^emil-compare]: [Official GitHub API comparison for `emilkowalski/skills`](https://api.github.com/repos/emilkowalski/skills/compare/d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7...d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7).
[^emil-tree]: [Current `emilkowalski/skills` skill tree](https://github.com/emilkowalski/skills/tree/d23d7f88a2e21c9e4b1418c7abe420f5c1052ba7/skills).
[^punk-compare]: [Official GitHub API comparison for `adrianpunk/Punk-Skill`](https://api.github.com/repos/adrianpunk/Punk-Skill/compare/50ea29b65b98788f9ed1df62818dbe530855bfb3...50ea29b65b98788f9ed1df62818dbe530855bfb3).
[^punk-tree]: [Current `adrianpunk/Punk-Skill` skill tree](https://github.com/adrianpunk/Punk-Skill/tree/50ea29b65b98788f9ed1df62818dbe530855bfb3/skills).
