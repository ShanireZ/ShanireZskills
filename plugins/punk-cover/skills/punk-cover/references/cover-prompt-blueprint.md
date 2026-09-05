# Punk Cover Prompt Blueprint

This blueprint defines the complete cover-prompt shape used by `punk-cover`.

`styles/{style-id}/STYLE.md` is a reusable visual style atom. This blueprint is the cover shape. The final `prompts/01-cover.md` must compile the selected style atom into this cover shape and read like one complete image-generation prompt.

Do not paste this blueprint verbatim with empty placeholders. Fill it with derived article fields and selected style anchors.

An image prompt cannot guarantee exact rendered text. When exact copy is required, this blueprint describes a no-text base artwork plus a deterministic typography layer. The workflow must validate the actual final pixels separately.

## Required Final Prompt Structure

```text
# {style name} cover prompt

You are a top-tier cover art director, editorial visual designer, typography designer, and image-generation prompt director.

Create one single {platform} cover image with aspect ratio {ratio}.

The cover must use the selected visual style: {style name} / {style id}.
This style is not a decorative filter. Every major cover decision must be implemented through this style's visual language.

Production mode: {prompt_only / deterministic_text_overlay / one_stage_generated_text}

## Copy Provenance and Approval

- Source-fact summary for visual reasoning only: {source_fact_summary}
- Approved exact main visual title: {approved_main_visual_title_or_none}
- Approved exact complete title, if separately displayed: {approved_complete_title_or_none}
- Approved exact subtitle: {approved_subtitle_or_none}
- Other approved visible copy: {approved_other_visible_copy_or_none}

Only the approved fields above may become visible text. Preserve their characters, punctuation, letter case, and wording exactly. Source facts and model suggestions are not visible copy. Do not invent, translate, normalize, shorten, expand, or silently substitute text.
Do not duplicate one approved title into both A-layer and B-layer unless the user approved both visible instances.

## Input

- Title/topic: {title_or_topic}
- Title hierarchy:
  - A-layer / main visual title: {approved_main_visual_title_or_none}
  - B-layer / complete title: {approved_complete_title_or_none}
  - C-layer / subtitle or small text: {approved_subtitle_or_none}
- Platform: {platform}
- Aspect ratio: {ratio}
- Output dimensions: {output_dimensions_or_auto}
- Output mode: {single_image_or_one_member_of_multi_size_suite}
- Language: {language}
- Use case: {use_case}
- Short context summary: {summary}
- Visual subject: {visual_subject}
- Audience: {audience}
- Mood: {mood}
- Visual metaphor: {metaphor}
- Banned elements: {banned_elements}

## Content Understanding

Understand the source material before composing the image, but do not output analysis in the image.

Use only derived fields from the article. Do not paste the original article body into the image, prompt, metadata, or small text system.

The cover must communicate:

1. What the topic is at first glance.
2. What the core tension, insight, or metaphor is at second glance.
3. Why this cover belongs to the selected visual style, not a generic cover template.

## Cover Objective

Generate a deliberate editorial cover, not a generic illustration, PPT cover, course cover, advertisement, or information card.

The main title must be complete, accurate, and clearly readable. If the approved copy uses a long-title hierarchy, use the hierarchy above:

- A-layer: the exact approved main visual title.
- B-layer: the exact approved complete title, when separately present.
- C-layer: the exact approved subtitle, when present.

Do not create an A-layer, B-layer, or C-layer from source material on your own. An automatically derived short title, subtitle, translation, context line, or label must be approved before this prompt is compiled.

## Text Rendering Path

Follow only the selected production mode:

- `deterministic_text_overlay`: generate the base artwork with no letters, glyphs, pseudo-text, logos, labels, or watermarks. Reserve crop-safe, visually integrated regions for the approved copy. After generation, a deterministic typography compositor places the approved strings using the selected style's type behavior, geometry, materials, and hierarchy.
- `one_stage_generated_text`: render only the approved exact strings and no other text. Keep all text inside safe margins and free from occlusion. This mode carries acknowledged spelling and cropping risk and still requires inspection of the actual output.
- `prompt_only`: prepare this production prompt but make no claim about generated text. Delivery status is `generated text: not tested`.

For text-as-object styles, the base artwork must reserve the structural space, surfaces, depth cues, masks, or paths needed for the deterministic type layer. Do not replace style-native typography with a generic caption.

## Style Application

Apply the selected style's non-negotiable anchors:

- Style anchors: {style_anchors}
- Cover-shape adaptation: {cover_shape_adaptation}
- Must preserve: {must_preserve}
- Style-specific avoid list: {avoid_when_applying_to_cover}

These anchors must visibly affect:

1. Main title treatment.
2. Visual subject construction.
3. Background or spatial system.
4. Supporting text and label system.
5. Texture, material, color, or rendering method.
6. The visual metaphor.

Do not mention a style trait unless it is actually visible in the final image.

## Composition

Design a cover-specific composition using the selected style.

Define:

- Primary visual center: {primary_visual_center}
- Secondary visual elements: {secondary_visual_elements}
- Background or space system: {background_space_system}
- Foreground/background layering: {layering_strategy}
- Reading path: {reading_path}
- Shareability constraint: the topic must be legible in a fast social feed.

## Image-Text Relationship

The title, subject, and style must be fused.

The title must not be a caption pasted on top of an unrelated image. The selected style must determine how the text exists in the scene:

- Where the main title lives.
- How the subtitle is carried.
- How labels, dates, tags, or small text behave.
- How images, objects, textures, or geometry interact with letterforms.

Any label, date, tag, note, or supporting line must be present in the approved-visible-copy manifest. If it is not approved, omit it even when the selected style atom normally permits or suggests it.

## Typography

Use typography appropriate to {style name}.

Rules:

- Render only approved visible copy and preserve every character exactly.
- Keep the main title readable.
- Do not crop, misspell, or over-distort key text.
- Use only a small amount of supporting text.
- Supporting text must deepen the cover concept, not become random filler.

## Color, Material, and Texture

Use the selected style's color, material, and texture logic:

{color_material_texture_rules}

The result must feel like a finished cover artwork in this style, not a style word applied superficially.

## Negative Constraints

Avoid:

- {banned_elements}
- Generic cover layout.
- PPT or course-cover feel.
- E-commerce advertisement feel.
- Unrelated decoration.
- Missing or unreadable main title.
- Long article text copied into the image.
- Style-specific failures: {avoid_when_applying_to_cover}

## Final Standard

Generate only one final image.

Do not output explanations, alternatives, grids, contact sheets, or multi-option compositions.

The final image must satisfy all of these:

1. It is clearly a {platform} cover at {ratio}.
2. It communicates the article topic quickly.
3. It uses the selected style as the visible organizing language.
4. The main title is readable and accurate.
5. The visual metaphor is present and style-native.
6. The result has the completeness and specificity of a legacy full cover prompt, while keeping the selected style reusable as an independent atom.
```

## Compilation Notes

- Rewrite the blueprint into a natural final prompt. Do not leave meta-instructions like `{primary_visual_center}` unresolved.
- For a multi-size suite, compile this blueprint once per target ratio or dimension. Each compiled prompt must request exactly one independently composed image and identify its target dimensions; do not ask one prompt to create a grid or contact sheet.
- Preserve the suite's content, metaphor, style identity, material, and palette across targets, while recomposing scale, typography, whitespace, reading direction, and spatial behavior for each ratio.
- Use the selected `META.md` metadata when it provides structured fields.
- Use `STYLE.md` to recover style language that is not yet structured in metadata.
- The final prompt may add style-specific sections when needed, but must not add another style.
- The final prompt should be longer and more complete than the raw style atom, because it includes cover shape, title hierarchy, and content adaptation.
- Copy approved visible strings exactly. Style placeholders or instructions that propose automatic subtitles, short titles, translations, labels, dates, or editorial notes are subordinate to the approval manifest.
- Keep suggested but unapproved copy out of the compiled prompt.
- In deterministic mode, make the base-art instruction explicitly text-free and keep the overlay specification deterministic and style-native.
- A correct prompt or text-layer source is not evidence that final rendered text passed. The actual current-run artifact must be inspected under the parent workflow.
