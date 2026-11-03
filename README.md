# Academic Figures

English | [简体中文](README.zh-CN.md)

A Codex skill for turning manuscript content, research data, and existing figures into clear academic visuals. Use it to plan figures, create data plots and scientific illustrations, refine methodology diagrams, or integrate figures into a LaTeX paper.

Give it your research material and the task you want completed. It chooses a suitable rendering approach, preserves the scientific meaning, checks the exported result, and has each version reviewed by an independent first-time reader.

## Quick start

Download or clone this repository. To make the skill available across your projects, run the following from the repository root:

```bash
mkdir -p ~/.agents/skills
cp -R academic-figures ~/.agents/skills/
```

This is Codex's documented user-level skill location. If the skill does not appear, restart Codex. See the [official skills documentation](https://learn.chatgpt.com/docs/build-skills).

Open your research project in Codex and give it a task, replacing these example paths with your own:

```text
$academic-figures Read paper.tex and inspect the existing figures in figures/. Identify where an additional figure would materially help the reader, then create the necessary figures. Use moderate information density, preserve method differences and qualifications, and inspect the final exports.
```

Provide the relevant data or plotting source when you want empirical plots. You can also supply a reference image, target figure width, or preferred renderer.

To try it without installing, ask Codex to read the repository's `academic-figures/SKILL.md` by its full local path before carrying out your task.

You can request just one stage:

- **Plan:** propose the content and layout without generating a figure.
- **Generate:** create a figure from the supplied material and improve it through review rounds.
- **Critique:** run one independent review round on an existing figure without editing it.
- **Refine:** change an existing figure while preserving its scientific content, then review the result.
- **Integrate:** add a selected figure and caption to a specified LaTeX manuscript.

## How it chooses what to draw

When asked to choose figures for a paper, the skill first checks the manuscript and existing figures. Each proposed figure should answer a distinct reader question and add useful visual explanation. It avoids duplicate coverage and does not require a fixed number of figures or one figure per section.

The defaults are one clear main message per figure and moderate information density. Related content can share a figure; separate messages or crowded layouts may call for separate figures. Supporting detail can move to the caption while retaining technical meaning and qualifications. Your requested scope, figure count, and density take precedence over these defaults.

| Content                                                  | Default rendering approach                                   |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| Data and quantitative results                            | The project's plotting tools, such as R or Python/Matplotlib |
| Precise methods, equations, and structural relationships | Editable SVG, TikZ, or an existing suitable workflow         |
| Scientific illustrations and conceptual imagery          | Codex's built-in image generation and editing                |

**Does it use GPT Image 2?** Yes, for the illustration route when built-in image generation is available. [OpenAI&#39;s current documentation](https://learn.chatgpt.com/docs/image-generation) identifies the built-in model as `gpt-image-2`. The skill invokes that tool without pinning a model version and does not require a separate API key. Data plots and precision-dependent diagrams use the code or vector route above. If a required tool is unavailable, the skill reports the limitation.

## Set the style and level of detail

Supply a reference figure and say which features you want to carry over: palette, line treatment, typography, layout, or information density. The skill adapts those choices to your paper rather than treating the reference's scientific content as evidence.

For example, attach a reference image and ask:

```text
$academic-figures Create one overview figure for methods.tex. Use the attached figure as a style reference for its muted palette and clean line treatment. Keep the information density moderate and the labels short. Match the manuscript typography, preserve the differences between methods, and put supporting explanations in the caption. Target a width of 6.5 inches.
```

Without a reference, it follows the manuscript's typography and dimensions, using restrained colors and readable labels. No single font, illustration style, or fixed layout is required.

## How figures are reviewed

The person who draws a figure knows what it is meant to say, so they cannot easily tell whether a newcomer gets it at a glance: the curse of knowledge. The skill therefore has every version reviewed by a fresh Codex subagent that has not seen the brief, the image prompt, or the conversation. The reviewer has the same problem in another form: it knows more than most readers and can decode what they could not. So it must show how it reached each conclusion, and Codex checks that reasoning against what the target readers can be assumed to know. Each round has four steps:

1. **Cold read.** The reviewer sees only the figure, downscaled to its printed size, and reports the main message it takes away, where its eye lands first, the order in which it reads the figure, and anything it cannot read. For each interpretation, it lists the visible elements it relied on and the conventions or background knowledge it brought in.
2. **Informed review.** Only then does it read the brief and compare what it took away with what you intended. It returns at most five findings, ranked blocker, major, or minor, each with a location and a concrete fix, and a verdict of ACCEPT or REVISE.
3. **Reasoning audit.** Codex checks each interpretation against the brief's list of what the target readers know. One that rests on knowledge outside that list, on a guess, or on text that is illegible at print size marks a point the figure fails to communicate, even when the reviewer got it right. The audit can only add findings and tighten the verdict.
4. **Response and revision.** Codex accepts or declines each finding with a reason, never at the expense of scientific accuracy, and revises the figure: it edits the current image for local fixes and regenerates it for layout problems.

Each new version gets a new reviewer, so every round is a genuine first reading. The loop ends when the reviewer accepts or after three rounds by default. If the same blocker survives two rounds in a row, Codex changes its approach or asks you instead of retrying the same prompt. You receive the best reviewed version and the record of every round.

You can adjust the loop in your request:

- "Run up to five review rounds."
- "Pause after each review so I can add feedback." Your feedback takes precedence over the reviewer's.
- "Critique figures/model.png." This runs one review round without redrawing.
- "The readers are psychology researchers who know regression but not structural equation models." This sets the knowledge the reasoning audit allows; otherwise Codex infers it from the venue and manuscript.

Subagents depend on Codex's `multi_agent` feature; `codex features list` shows whether it is enabled. Without subagents, the skill runs the reviewer as a separate `codex exec` process. If neither route works, it reviews the figure itself and labels that review as not independent.

## What you receive

For generation work, the skill keeps every version of the figure together with its editable source or exact image-generation prompt, relevant input references, the brief the reviewers checked against (`brief.md`), the record of each review round (`review/`), and concise notes. Vector workflows include a PDF export and PNG preview; generated artwork is retained as PNG.

Checks cover scientific fidelity, final-size readability, mathematical notation, grayscale clarity, and export defects. PDF delivery includes inspection of a preview rasterized from the final PDF. Manuscript integration is checked separately when requested and reported as unchecked if it was not inspected.

Plan-only and critique-only requests return findings in chat by default.

## Skill files

- [academic-figures/SKILL.md](academic-figures/SKILL.md): workflow and quality requirements.
- [academic-figures/references/review-protocol.md](academic-figures/references/review-protocol.md): instructions for the independent reviewer subagent.
- [academic-figures/agents/openai.yaml](academic-figures/agents/openai.yaml): display metadata and default invocation prompt.

These files are written in English. Image-generation prompts are composed for each task; their language is not currently fixed by the skill. This README and its Chinese translation describe the same workflow.
