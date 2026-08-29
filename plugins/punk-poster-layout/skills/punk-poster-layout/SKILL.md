---
name: punk-poster-layout
description: Choose, prompt, implement, and review composition for standalone posters, flyers, key visuals, and single-canvas editorial or poster-like sections. Use only when the request explicitly needs structural control of focal flow, hierarchy, grids, image-text relationships, or density. Do not trigger for ordinary landing pages, apps, dashboards, card grids, generic heroes, or article, social, and platform covers that do not explicitly request composition control.
---

# Punk Poster Layout

## Core Principle

Decide structure before styling. Style words describe how a poster looks; composition must state where the focal point sits, how large each layer is, how the eye moves, which elements form a group, how visual weight balances, and where space remains open.

Treat a grid differently by output medium:

- In an image-generation prompt, a grid is a soft structural constraint.
- In HTML/CSS, turn the same columns, rows, spans, gutters, alignment lines, and crop focal points into inspectable layout rules.

## Scope and Responsibility

- For an article, social, or platform cover, a dedicated cover workflow leads when one is available. Use this skill only as a subordinate structural step when composition control is explicitly needed.
- This skill does not own a visual-style library. Carry forward the style supplied by the user or upstream workflow; never select an external style atom on your own.
- If no style is supplied, a structure-only recommendation or prompt is valid. Ask for style only when the user requested a complete visual direction and the missing choice would materially change that direction.

## Resources

1. Read [references/composition-selector.md](references/composition-selector.md) to compare all 32 systems and choose a direction.
2. Read exactly the detailed catalog that contains the chosen primary composition:
   - 01–16: [references/focus-and-motion.md](references/focus-and-motion.md)
   - 17–32: [references/grid-and-density.md](references/grid-and-density.md)
3. Read [references/prompting-and-review.md](references/prompting-and-review.md) when writing a final prompt, implementing HTML/CSS, or reviewing an output.

Do not load both detailed catalogs after the composition is known unless the task genuinely combines one primary composition with one supporting relationship from the other half.

## Deliverable and Action Boundary

| Deliverable | Allowed action and completion condition | Keep explicit as unverified |
| --- | --- | --- |
| Recommendation | Read-only. Return one primary composition, its rationale, structural values, and constraints. Complete when the recommendation is usable without changing or generating an artifact. | Crop safety, responsive behavior, and rendered readability are `not tested`. |
| Prompt | Return the prompt by default; do not call an image tool. Complete when the prompt specifies the structure and factual copy exactly. | Rendering, generated text, crop safety, and responsive behavior are `not tested`. |
| Generated poster | Generate only when the user explicitly requests a generated poster. Save only when the user specifies a destination or the tool explicitly provides a current-turn artifact to save. Complete when that artifact is returned and inspected at the requested size. | Unrequested exports, alternate viewports, and model-rendered text that was not verified remain untested. |
| HTML/CSS | Modify only an explicitly identified target. Without one, return a self-contained example or implementation specification. Complete when the requested code is delivered or implemented and the checks actually run are named. | Any viewport, zoom level, crop, contrast, or interaction not exercised remains untested. |
| Review | Require an actual artifact and its target viewport or output size. Report only observable evidence and do not auto-fix it. Complete when findings, affected variables, and measured corrections are reported. | Anything outside the supplied artifact and inspected viewport remains untested. |

## Required Inputs

- Never invent factual copy or identity: title, date, location, ticketing details, names, prices, URLs, logos, and brand marks must come from the user or an authoritative upstream brief.
- If the aspect ratio is missing and different ratios would materially change the composition, ask. Use a stated default only when the user explicitly says `auto`.
- Do not turn every missing field into a hard stop. Infer only non-factual structural defaults that do not change the brief, and state them.
- For HTML/CSS without a named project target, return a self-contained example or specification instead of editing the workspace.
- For review, stop and request the artifact plus the intended viewport or output size when either is missing.

## Workflow

1. Establish the deliverable: image prompt, generated poster, HTML/CSS poster, single-canvas editorial or poster-like section, composition recommendation, or review.
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
   - For a review, identify the broken structural variable from observable evidence and propose a measurable correction without changing the artifact.
6. Run the relevant review checklist before finishing.

Ask when required factual copy is unavailable, when a missing ratio materially changes the composition, or when another missing choice cannot be inferred without changing the brief. Otherwise choose only a defensible non-factual structural default and state it briefly.

## Composition Rules

- Use one primary focal point and a legible three-level hierarchy.
- Give every split, panel, card, or repeated element an explicit relationship to the rest.
- Preserve one or two shared alignment lines even in asymmetric layouts.
- Use ratios such as `60/40`, `62/38`, or `70/30` when equal division would erase hierarchy.
- Keep experimental work constrained: establish the rule first, then use a small number of related, controlled breaks—usually one primary break element or focal cluster. In composition 30, the title and main visual may form two related breaks that serve one hierarchy.
- Keep key faces, hands, product marks, title strokes, dates, and CTAs inside safe crop zones.
- Preserve semantic DOM order, information relationships, and hierarchy across responsive sizes; geometry may change when minimum type size, minimum unit width, crop safety, or interaction needs would otherwise fail.
- In HTML/CSS, keep one accessible copy of each text, provide appropriate image alternatives, use link/button semantics for CTAs, retain visible keyboard focus, and test narrow viewports and 200% zoom.

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
- the preflight/spec-lint result and an explicit list of `not tested` artifact checks; include artifact-review findings only when an actual artifact was inspected.

For HTML/CSS implementation, also include or implement a switchable debugging overlay when a grid or path is central to the design, then keep that overlay off in the finished output.
