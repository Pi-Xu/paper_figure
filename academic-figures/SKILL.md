---
name: academic-figures
description: Plan, generate, critique, refine, and integrate academic paper figures, including data plots, methodology diagrams, scientific illustrations, and LaTeX figure blocks. Use for publication-oriented figure work grounded in manuscript text, data, or existing figures.
---

# Academic figures

Turn research material into accurate, readable paper figures using Codex's file access, shell, image inspection, native image generation, and subagents. Use the tools available in the current session; no external MCP server, provider SDK, or API key is required by this workflow.

Inspired by [PaperBanana](https://arxiv.org/abs/2601.23265): use references, planning, styling, visualization, and critique as stages within one Codex workflow. Critique runs in a fresh reviewer subagent to counter the curse of knowledge: a figure must work for a first-time reader, and its author, who already knows what it is meant to say, cannot judge that. A reference database is unnecessary.

If you were started as a figure reviewer, follow [references/review-protocol.md](references/review-protocol.md) alone.

## Select the requested work

- **Plan:** propose the figure's message, content, layout, and rendering approach. Return the brief in chat unless a saved plan is requested; do not generate a figure yet.
- **Generate:** establish a brief, render, improve the figure through the review loop, and deliver it with its source or prompt.
- **Critique:** run one review round on the supplied figure, using the available scientific context for its brief. Return prioritized findings and concrete fixes; do not edit files or regenerate the figure. Distinguish visible defects from scientific claims that cannot be verified without source material.
- **Refine:** inspect the current figure and editable source, apply the requested changes, and run the review loop on the result. Preserve scientific content and unrelated design choices.
- **Integrate:** add the selected figure and caption to the requested LaTeX manuscript, or provide a snippet if no target manuscript is specified. Reuse an existing acceptable figure without regenerating it.

Perform only the requested stages. For an end-to-end request, proceed through the workflow without introducing approval checkpoints. Ask only when a missing scientific fact, ambiguous target, or material preference blocks a correct result.

## Establish the brief

Read the relevant manuscript passages, data, existing plotting code, nearby figures, and LaTeX conventions before choosing a design. Prefer user-supplied and local references. Use native browsing for additional references when requested or when it materially helps; reference retrieval is optional. Record external sources actually used. When style references are supplied, identify useful choices in palette, line treatment, typography, layout, and information density, adapting them to the manuscript's content and dimensions. Style examples guide presentation, not the scientific claims.

When asked to choose figures for a manuscript, review existing figures first and identify content that benefits materially from visual explanation. State the reader question each proposed figure answers and avoid duplicating existing coverage. Default to only the figures needed for distinct explanatory purposes, without a fixed count or one figure per section. Group related content when it supports one main message; split it when separate messages or readability at the final size require it. Respect user-specified figure counts and scope; a request to refine one figure does not authorize adding others.

Default to moderate information density: give each figure one clear main message, using only the panels, steps, labels, and formulas needed to explain it and preserve essential distinctions. Apply the caption and text-reduction guidance below for supporting detail. Explicit user preferences for style and density override these defaults while preserving scientific accuracy and readability.

Write a concise brief covering:

- The figure's main message, phrased as the one-sentence takeaway a first-time reader should get, and the evidence or method supporting it.
- The target reader, as the user describes them or as inferred from the venue and manuscript: their field, and the notation, terms, and visual conventions they can be assumed to know, listed explicitly. Anything unlisted, including terms the paper itself introduces, counts as unknown to them.
- Panels, required labels with their exact text, notation, and the exact relationships or quantities shown.
- The entry point the reader should look at first, reading order, visual hierarchy, intended information density, and meaningful color/grouping choices.
- Target width, aspect ratio, fonts, selected renderer, and a draft caption.

Save the brief as `brief.md` in the figure directory before the first review round. The reviewer checks the figure against it, so it holds only what the figure must communicate; image prompts, provenance, and working notes go in `notes.md`. Fix the target-reader list before the first round and change it only when the user corrects it, so a failed review cannot be explained away afterwards.

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

## Inspect each render

Render and view the actual output: inspect the PNG preview and, when delivering PDF, a separate preview rasterized from the final PDF. Preview SVG exports through rasterization as well; source inspection alone is insufficient. Check the overall figure at its intended publication width and enlarge mathematical symbols, formula backgrounds, and arrows for detail. If the tools cannot render or display it, distinguish source inspection from visual verification and report the unchecked part.

Check these dimensions against the brief and source material:

- **Scientific fidelity:** values, units, equations, panel meanings, arrow direction, and caption claims.
- **Completeness:** required elements, labels, legends, and definitions; no unexplained symbols or extra generated elements.
- **Readability:** font size, visual hierarchy, reading order, spacing, alignment, and crossing connectors; formula baselines, visual centering, and spacing relative to surrounding text. After font or text-density changes, recheck box sizes, whitespace, alignment, and arrows, removing unnecessary height.
- **Export quality:** clipping (including formulas), substituted fonts, missing or inconsistent glyphs, misplaced subscripts/superscripts, unintended formula backgrounds, changed arrowheads, margins, contrast, and raster resolution at the intended size. Verify the fonts actually used by the renderer/exporter through resolved-font or exported-font information as applicable and visual inspection; SVG font-family declarations alone do not establish correct font use.
- **Accessibility:** inspect a grayscale preview; methods and groups must remain identifiable through labels and structure, with line styles or markers where needed.

Prioritize scientific errors, then readability, then cosmetic polish. Before each review round, make one correction pass for the defects you find and re-render affected outputs, so the reviewer's attention goes to what you cannot judge yourself: how the figure reads to someone who has not seen the brief. If issues remain when the review loop ends, deliver the best reviewed draft with explicit unresolved findings rather than claiming it is ready for publication.

## Review loop

A fresh reviewer subagent judges each version, you audit how it reached its reading, and then you triage its findings and revise. Start a new reviewer every round and hand it only file paths: a reviewer that has seen an earlier version, the brief, or this conversation is no longer a first-time reader, and its first answer must be a cold read of the figure alone. Number rounds `r1`, `r2`, … per figure, continuing across requests.

### Run a round

1. Save the version as `<name>-vN.<ext>` beside earlier versions. Make `review/vN-print.png`, a copy downscaled to the final printed width at about 150 pixels per inch (3.35 in ≈ 500 px; 6.5 in ≈ 975 px), rasterized from the final PDF for vector outputs. Add `review/vN-gray.png` when color carries meaning.
2. Start the reviewer with Codex's native subagent tool (for example `spawn_agent`) as a fresh agent whose context holds only this message; leave any option to fork or share your conversation off:

   > You are an independent reviewer of a figure for an academic paper. Read `<absolute path of references/review-protocol.md>` and do Phase A. Figure: `<absolute path of review/vN-print.png>`.

   Save the reply verbatim as Phase A of `review/rN.md`.
3. Send the same reviewer a second message (for example with `send_input`), leaving out items that do not exist yet:

   > Do Phase B for round N. Brief: `<path>`. Full-resolution figure: `<path>`. Grayscale: `<path>`. Previous version: `<path>`. Previous review with audit and author response: `<path of review/r(N-1).md>`.

   Save the reply verbatim as Phase B of `review/rN.md`, then close the reviewer. If you cannot message the same reviewer again, start a second fresh reviewer for Phase B and paste the saved Phase A into its message.

These two messages are the whole hand-off. Your prompts, notes, summaries of the figure, suspicions, and account of what changed stay with you; the reviewer judges the files directly.

If subagents are unavailable or cannot view images, run each phase as a separate read-only `codex exec` process with that phase's images attached (see `codex exec --help`; it needs network access, so request approval if the sandbox blocks it), and pass the saved Phase A to the Phase B process. If neither route works, do the round yourself with the same protocol, writing Phase A from the print-size preview before rereading the brief, and label the round `self-review, not independent` in `review/rN.md` and in the final report.

For Critique, run one round on the supplied figure without revising it. Write the brief from the manuscript and the user's account of the figure's purpose, marking unknown intent as unknown; keep the brief, previews, and review in a temporary directory unless a report was requested; and return the reviewer's findings together with the reasoning audit below.

### Audit the reasoning

The reviewer knows far more than most readers and cannot un-know it, so its reading can succeed on expertise or guesswork where the figure itself fails. Phase A therefore ends with an inference trace giving, for each interpretation, the visible evidence and the knowledge it used. Audit every row against the print-size preview and the target-reader list in `brief.md`:

- **Evidence:** the cited element exists, is legible at print size, and reads as quoted.
- **Knowledge:** every convention or fact the row uses is on the target-reader list or defined in the draft caption. Count anything else as unknown to readers, however obvious it seems to you.
- **Inference:** the interpretation follows from that evidence and knowledge; a row whose basis is a guess fails.

Each failed row is a point the figure does not carry for its readers. Add it as a finding with ID `rN-aK`, graded by the severity rules in [references/review-protocol.md](references/review-protocol.md), with a fix that shows or labels the point in the figure or defines it in the caption. If the main message rests on a failed row, a MATCH becomes PARTIAL and the verdict becomes REVISE. The audit can only add findings and lower the verdict, never raise it. Record it as a table in `review/rN.md` between Phase B and the author response.

### Respond and revise

Triage every finding in an "Author response" table at the end of `review/rN.md`: accepted, with the change made, or declined, with a reason grounded in the brief, the scientific content, or the user's instructions. Decline any fix that would change scientific content, terminology, or claim strength, and solve the underlying readability problem another way. User feedback outranks the reviewer; record it in the same table. A round is complete when `review/rN.md` holds both phases verbatim, the reasoning audit, and a response to every finding.

Change only what accepted findings require, so the parts that work survive. For generated artwork, make local fixes, such as a label, an arrow, or a color, by editing the current version as the edit target and naming what must stay unchanged; regenerate from the updated full prompt only for layout, hierarchy, or reading-order problems. Record each version's exact prompt and method in `notes.md`. For code-rendered figures, change the source and re-export.

### Stop

- Stop when the verdict, after the reasoning audit, is ACCEPT.
- Run at most three rounds by default, which allows two revisions; a user-specified count overrides this. Deliver only reviewed versions.
- When the same blocker survives two consecutive rounds, change strategy rather than retrying the prompt: simplify the element, switch between editing and regenerating, or propose the code route. Ask the user if none fits.
- Deliver the best reviewed version: when a reviewer judges the newest version WORSE than the previous one, deliver the previous one. Report the blocker and major findings still open as unresolved.
- Run rounds without pausing unless the user asks to give feedback each round; then, after each review, show the verdict, the findings, and your planned response, and wait.

## Save and integrate

Follow the project's figure directories and naming. Otherwise use `figures/<figure-name>/`. Keep these artifacts together for generation work:

- Editable source or exact image-generation prompt, plus references to the input data/material.
- `brief.md`, the contract each reviewer checks against.
- Every reviewed version, the selected final figure, and a PNG preview for vector outputs. The final PNG also serves as the preview for artwork.
- `review/`, holding the print-size previews and one `rN.md` per round.
- One compact `notes.md` containing provenance, each version's exact prompt and revision method, the reproduction command, a one-line outcome per review round, and unresolved issues.

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

Finish with links to the selected figure and source/prompt, the LaTeX snippet or updated manuscript when applicable, a brief account of checks performed, one line per review round (glance verdict after the reasoning audit, open blocker and major findings, what changed), and any material unresolved issues. Mark any round that was not independent. Distinguish standalone figure previews from checks of the figure in the compiled manuscript, explicitly stating when manuscript integration was not inspected. Do not claim reproducibility or visual validation beyond what was actually checked.

## Example requests

- “Use $academic-figures to plan a methodology figure from methods.tex.”
- “Use $academic-figures to plot results.csv with the paper's existing uncertainty definition.”
- “Use $academic-figures to critique figures/model.png against the methods section.”
- “Use $academic-figures to generate the overview figure from methods.tex with up to two review rounds, pausing after each review for my feedback.”
- “Use $academic-figures to simplify this diagram while preserving every edge and label.”
- “Use $academic-figures to generate a conceptual illustration of a porous membrane separating large and small particles.”
- “Use $academic-figures to integrate figures/method/method.pdf into paper.tex.”
