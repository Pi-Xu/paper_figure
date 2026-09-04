# Academic Figures

English | [简体中文](README.zh-CN.md)

A Codex skill for turning manuscript content, research data, and existing figures into clear academic visuals. Use it to plan figures, create data plots and scientific illustrations, refine methodology diagrams, or integrate figures into a LaTeX paper.

Give it your research material and the task you want completed. It chooses a suitable rendering approach, preserves the scientific meaning, and checks the exported result.

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
- **Generate:** create, inspect, and refine a figure from the supplied material.
- **Critique:** review an existing figure without editing it.
- **Refine:** change an existing figure while preserving its scientific content.
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

## What you receive

For generation work, the skill keeps the figure together with its editable source or exact image-generation prompt, relevant input references, and concise review notes. Vector workflows include a PDF export and PNG preview; generated artwork is retained as PNG.

Checks cover scientific fidelity, final-size readability, mathematical notation, grayscale clarity, and export defects. PDF delivery includes inspection of a preview rasterized from the final PDF. Manuscript integration is checked separately when requested and reported as unchecked if it was not inspected.

Plan-only and critique-only requests return findings in chat by default.

## Skill files

- [academic-figures/SKILL.md](academic-figures/SKILL.md): workflow and quality requirements.
- [academic-figures/agents/openai.yaml](academic-figures/agents/openai.yaml): display metadata and default invocation prompt.

Both files are written in English. Image-generation prompts are composed for each task; their language is not currently fixed by the skill. This README and its Chinese translation describe the same workflow.
