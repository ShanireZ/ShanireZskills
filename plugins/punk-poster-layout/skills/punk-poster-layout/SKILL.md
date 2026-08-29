---
name: punk-poster-layout
description: Choose, prompt, implement, and review poster composition with 32 named layout systems for image-generation prompts and HTML/CSS posters. Use when a request involves poster layout, focal flow, grid systems, information hierarchy, image-text relationships, visual density, or poster-like landing-page composition; this skill controls structure rather than visual style.
---

# Punk Poster Layout

## Core Principle

Decide structure before styling. Style words describe how a poster looks; composition must state where the focal point sits, how large each layer is, how the eye moves, which elements form a group, how visual weight balances, and where space remains open.

Treat a grid differently by output medium:

- In an image-generation prompt, a grid is a soft structural constraint.
- In HTML/CSS, turn the same columns, rows, spans, gutters, alignment lines, and crop focal points into inspectable layout rules.

## Resources

1. Read [references/composition-selector.md](references/composition-selector.md) to compare all 32 systems and choose a direction.
2. Read exactly the detailed catalog that contains the chosen primary composition:
   - 01–16: [references/focus-and-motion.md](references/focus-and-motion.md)
   - 17–32: [references/grid-and-density.md](references/grid-and-density.md)
3. Read [references/prompting-and-review.md](references/prompting-and-review.md) when writing a final prompt, implementing HTML/CSS, or reviewing an output.

Do not load both detailed catalogs after the composition is known unless the task genuinely combines one primary composition with one supporting relationship from the other half.

## Workflow

1. Establish the deliverable: image prompt, generated poster, HTML/CSS poster, poster-like landing-page section, composition recommendation, or review.
2. Derive the structural inputs from the brief:
   - aspect ratio and target size;
   - title, supporting information, date/location/CTA, and their priority;
   - primary visual subject and crop-sensitive details;
   - intended first focal point and final reading destination;
   - content density, emotional tone, and required negative space;
   - required comparisons, sequences, or repeated items.
3. Use the selector to choose exactly one primary composition. Add at most one subordinate relationship when it solves a separate need, such as `Rule of Thirds + S-Curve` or `Modular Grid + one broken-grid title`.
4. State the structural brief with concrete positions, proportions, spans, paths, and constraints. Prefer `four columns, 20px gutters, title spans three columns` over `clean grid`; prefer `65%–80% active negative space` over `minimal`.
5. Produce the requested deliverable:
   - For an image prompt, integrate the structure into one complete prompt rather than appending a list of composition keywords.
   - For HTML/CSS, encode the same structure with Grid/Flexbox, explicit spans, shared alignment lines, responsive tokens, `object-fit`, and `object-position`.
   - For a review, identify the broken structural variable first and propose a measurable correction.
6. Run the relevant review checklist before finishing.

Ask a question only when a missing choice would materially change the composition and cannot be inferred. Otherwise choose a defensible default and state it briefly.

## Composition Rules

- Use one primary focal point and a legible three-level hierarchy.
- Give every split, panel, card, or repeated element an explicit relationship to the rest.
- Preserve one or two shared alignment lines even in asymmetric layouts.
- Use ratios such as `60/40`, `62/38`, or `70/30` when equal division would erase hierarchy.
- Keep experimental work constrained: establish the rule first, then break it once.
- Keep key faces, hands, product marks, title strokes, dates, and CTAs inside safe crop zones.
- Preserve the composition relationship across responsive sizes; do not automatically collapse every poster into a generic single-column stack.

## Failure Modes

Do not:

- substitute vague style terms such as “高级、极简、电影感” for a composition;
- combine several unrelated composition names without positions or roles;
- make every corner, panel, card, or anomaly equally strong;
- expose construction lines in the final artwork unless they are an intentional visual element;
- rotate all body text merely because the composition uses a diagonal axis;
- use transparency alone to solve type-over-image contrast;
- treat random rotation or absolute positioning as collage or broken-grid structure;
- preserve a desktop screenshot while allowing the responsive relationship to collapse.

## Output Contract

For a recommendation or prompt, include:

- selected primary composition and any supporting relationship;
- a one-sentence reason tied to the content and reading path;
- focal point, hierarchy, balance, path, spacing, and crop rules;
- concrete layout values appropriate to the medium;
- a short negative-constraint block;
- the relevant review result.

For HTML/CSS implementation, also include or implement a switchable debugging overlay when a grid or path is central to the design, then keep that overlay off in the finished output.
