---
name: academic-figures
description: Plan, generate, critique, refine, and integrate academic paper figures, including data plots, methodology diagrams, scientific illustrations, and LaTeX figure blocks. Use for publication-oriented figure work grounded in manuscript text, data, or existing figures.
---

# Academic figures

Turn research material into accurate, readable paper figures using Codex's file access, shell, image inspection, and native image generation. Use the tools available in the current session; no external MCP server, provider SDK, or API key is required by this workflow.

Inspired by [PaperBanana](https://arxiv.org/abs/2601.23265): use references, planning, styling, visualization, and critique as stages within one Codex workflow. Separate agents and a reference database are unnecessary.

## Select the requested work

- **Plan:** propose the figure's message, content, layout, and rendering approach. Return the brief in chat unless a saved plan is requested; do not generate a figure yet.
- **Generate:** establish a brief, render, inspect, refine as needed, and deliver the figure with its source or prompt.
- **Critique:** inspect the supplied figure and available scientific context. Return prioritized findings and concrete fixes; do not edit files or regenerate the figure. Distinguish visible defects from scientific claims that cannot be verified without source material.
- **Refine:** inspect the current figure and editable source, apply the requested changes, and check the result. Preserve scientific content and unrelated design choices.
- **Integrate:** add the selected figure and caption to the requested LaTeX manuscript, or provide a snippet if no target manuscript is specified. Reuse an existing acceptable figure without regenerating it.

Perform only the requested stages. For an end-to-end request, proceed through the workflow without introducing approval checkpoints. Ask only when a missing scientific fact, ambiguous target, or material preference blocks a correct result.

## Establish the brief

Read the relevant manuscript passages, data, existing plotting code, nearby figures, and LaTeX conventions before choosing a design. Prefer user-supplied and local references. Use native browsing for additional references when requested or when it materially helps; reference retrieval is optional. Record external sources actually used. When style references are supplied, identify useful choices in palette, line treatment, typography, layout, and information density, adapting them to the manuscript's content and dimensions. Style examples guide presentation, not the scientific claims.

When asked to choose figures for a manuscript, review existing figures first and identify content that benefits materially from visual explanation. State the reader question each proposed figure answers and avoid duplicating existing coverage. Default to only the figures needed for distinct explanatory purposes, without a fixed count or one figure per section. Group related content when it supports one main message; split it when separate messages or readability at the final size require it. Respect user-specified figure counts and scope; a request to refine one figure does not authorize adding others.

Default to moderate information density: give each figure one clear main message, using only the panels, steps, labels, and formulas needed to explain it and preserve essential distinctions. Apply the caption and text-reduction guidance below for supporting detail. Explicit user preferences for style and density override these defaults while preserving scientific accuracy and readability.

Write a concise brief covering:

- The figure's main message and the evidence or method supporting it.
- Panels, required labels and notation, and the exact relationships or quantities shown.
- Reading order, visual hierarchy, intended information density, and meaningful color/grouping choices.
- Target width, aspect ratio, fonts, and selected renderer.

Inspect the paper's body-text and mathematical fonts before choosing figure typography. Prefer fonts that coordinate with the manuscript and remain clear at the target size; no single font is required for all academic figures. Use bold sparingly and avoid oversized titles, all-caps group headings, and excessive weight changes. Carry compatible style choices across figures in the same project.

Follow the paper's dimensions; set font sizes and spacing after establishing the figure width. If unspecified, start with a single-column width of 3.35 inches (about 85 mm), a white background, and restrained colors. For a methodology diagram about 6.5 inches wide, start with main labels at 8–9 pt and secondary labels no smaller than about 7.5–8 pt, adjusting for readability at the final size. These defaults and reference sizes are not venue requirements.

Never invent data, uncertainty intervals, scientific relationships, or results. Compute quantities from supplied data using the documented analysis. If data are missing, complete the layout plan and ask for the needed data before drawing empirical marks. Use synthetic data only when requested, and identify it as illustrative in the figure and notes. Do not infer an arrow's direction or causal meaning from appearance alone.

## Choose and use the renderer

| Figure type | Default approach | Retain |
| --- | --- | --- |
| Quantitative plot | Existing project plotting stack; otherwise Python/Matplotlib | Data references, runnable plotting code, PDF, PNG preview |
| Precise methodology or structural diagram | Editable SVG; preserve existing TikZ or other suitable source | Editable source, PDF export, PNG preview |
| Illustrative artwork | Built-in image generation and editing | Exact prompt, reference provenance, selected PNG |

Respect an explicit renderer choice. Preserve established R, TikZ, or other project workflows when modifying their figures. Keep figure-specific scripts with project outputs; do not create a general rendering framework. Use only the dependencies needed for the chosen route and check that the relevant renderer/exporter is available.

### Plots and precise diagrams

Keep values, labels, equations, connections, and coordinates under code control. Use direct PDF/PNG export from the plotting stack. For SVG, use an available local exporter such as Inkscape and apply the export checks below. A raster embedded in a PDF remains raster.

Keep methodology diagrams focused on key steps, short phrases, and explicit connections rather than manuscript paragraphs. Move repeated explanations, symbol definitions, and necessary detail into the caption where appropriate; the figure and caption together must retain technical meaning, method differences, and qualifications. Shortening must not change terminology, formulas, causal relationships, or claim strength. Resolve crowding by removing repetition and shortening labels first, then adjusting layout and font size; do not rely on shrinking text alone.

Use consistent mathematical typesetting for Greek symbols such as θ, hatted parameters, subscripts, superscripts, and formula variables, using the project's TeX-compatible tools when needed. Do not assume Unicode Greek letters in a text font match the mathematical font. Set variables in mathematical italics and parentheses and operators according to mathematical conventions. Math may be converted to vector paths for consistent display, but retain editable formula source.

Embed SVG formulas with transparent backgrounds by default so they do not cover panel colors. Inspect and remove actual background elements using the SVG structure and styles, rather than relying on string replacements sensitive to whitespace or style syntax.

Preserve units, scales, category ordering, and uncertainty definitions. Distinguish observations, estimates, and uncertainty in legends or captions. Use line styles, markers, or labels as well as color where groups need to remain distinguishable.

### Native illustrations

Use the built-in image-generation tool and its current tool schema. Consult the installed `imagegen` skill when available for its native workflow; do not depend on an absolute skill path or its API/CLI fallback. Do not request provider credentials or silently switch to a separate API service.

Build the prompt from the brief: intended scientific meaning, required objects, composition, style, exact labels, and constraints. Identify each input image as a reference or edit target. Inspect local edit targets with the native image viewer before editing. Check the generated output against the brief, especially text, duplicated objects, and connections; a plausible-looking image is not scientific verification.

Use code for empirical charts and precision-dependent mathematical or structural content. Generated artwork may support those figures but must not supply invented measurements or relations. Copy the selected generated image into the project; do not reference an asset that exists only in the tool's output directory. Save the exact prompt and reference paths, while making clear that regeneration is not deterministic.

If native generation is unavailable or fails, report that limitation. Continue independent planning or critique and offer a suitable code-based alternative; do not substitute a sketch for explicitly requested artwork without agreement.

## Inspect and refine

Render and view the actual output: inspect the PNG preview and, when delivering PDF, a separate preview rasterized from the final PDF. Preview SVG exports through rasterization as well; source inspection alone is insufficient. Check the overall figure at its intended publication width and enlarge mathematical symbols, formula backgrounds, and arrows for detail. If the tools cannot render or display it, distinguish source inspection from visual verification and report the unchecked part.

Check these dimensions against the brief and source material:

- **Scientific fidelity:** values, units, equations, panel meanings, arrow direction, and caption claims.
- **Completeness:** required elements, labels, legends, and definitions; no unexplained symbols or extra generated elements.
- **Readability:** font size, visual hierarchy, reading order, spacing, alignment, and crossing connectors; formula baselines, visual centering, and spacing relative to surrounding text. After font or text-density changes, recheck box sizes, whitespace, alignment, and arrows, removing unnecessary height.
- **Export quality:** clipping (including formulas), substituted fonts, missing or inconsistent glyphs, misplaced subscripts/superscripts, unintended formula backgrounds, changed arrowheads, margins, contrast, and raster resolution at the intended size. Verify the fonts actually used by the renderer/exporter through resolved-font or exported-font information as applicable and visual inspection; SVG font-family declarations alone do not establish correct font use.
- **Accessibility:** inspect a grayscale preview; methods and groups must remain identifiable through labels and structure, with line styles or markers where needed.

Prioritize scientific errors, then readability, then cosmetic polish. Make targeted corrections and re-render affected outputs. By default, use at most two refinement rounds after the first rendering and stop earlier if no material issues remain. User-requested additional iterations override this default. If issues remain at the limit, deliver the best current draft with explicit unresolved findings rather than claiming it is ready for publication.

## Save and integrate

Follow the project's figure directories and naming. Otherwise use `figures/<figure-name>/`. Keep these artifacts together for generation work:

- Editable source or exact image-generation prompt, plus references to the input data/material.
- Selected final figure and a PNG preview for vector outputs. The final PNG also serves as the preview for artwork.
- One compact `notes.md` containing the brief, provenance, reproduction command or prompt, review findings, and unresolved issues.

Do not copy large datasets unnecessarily. Use project-relative paths where practical. For refinement, preserve the original and use a sibling version such as `method-v2.svg` unless replacement was requested; keep its corresponding exports aligned. For critique-only work, keep findings in chat unless a report was requested.

When LaTeX integration is requested, use the manuscript's document class, figure placement conventions, caption style, and labels. Include a PDF for vector figures or PNG for raster artwork; avoid adding an SVG build dependency solely for inclusion. Check paths relative to the document's build root, avoid duplicate labels, and place `\label` after `\caption`. Explain symbols and uncertainty in the caption without adding unsupported conclusions.

For a simple single-column figure, adapt this example:

```latex
% Preamble, if not already present: \usepackage{graphicx}
\begin{figure}[tbp]
  \centering
  \includegraphics[width=\linewidth]{figures/method/method.pdf}
  \caption{Description of the depicted method and its notation.}
  \label{fig:method}
\end{figure}
```

Use `figure*` only when a two-column manuscript calls for a figure spanning both columns. If no manuscript target is specified, supply an adapted snippet with the deliverables. With an integration target, edit it and run the project's LaTeX build when available; inspect the resulting figure page. Report unavailable compilation or unrelated build failures accurately.

Finish with links to the selected figure and source/prompt, the LaTeX snippet or updated manuscript when applicable, a brief account of checks performed, and any material unresolved issues. Distinguish standalone figure previews from checks of the figure in the compiled manuscript, explicitly stating when manuscript integration was not inspected. Do not claim reproducibility or visual validation beyond what was actually checked.

## Example requests

- “Use $academic-figures to plan a methodology figure from methods.tex.”
- “Use $academic-figures to plot results.csv with the paper's existing uncertainty definition.”
- “Use $academic-figures to critique figures/model.png against the methods section.”
- “Use $academic-figures to simplify this diagram while preserving every edge and label.”
- “Use $academic-figures to generate a conceptual illustration of a porous membrane separating large and small particles.”
- “Use $academic-figures to integrate figures/method/method.pdf into paper.tex.”
