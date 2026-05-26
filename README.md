# MarkUp

MarkUp turns long-form English Markdown into portable HTML pages that people can scan quickly: sidebar contents, Mermaid diagrams, timelines, callouts, comparison cards, metric cards, decision matrices, risk registers, collapsibles, copyable code blocks, and print-friendly styling.

It is not a generic Markdown converter. It is an AI-agent skill: the agent reads the source document, chooses the right visual components, and writes one self-contained HTML file.

<p align="center">
  <img src="docs/preview-light.png" alt="MarkUp light theme preview" width="49%" />
  <img src="docs/preview-dark.png" alt="MarkUp dark theme preview" width="49%" />
</p>

## What It Does

Use MarkUp for:

- Plans and migration rollouts
- Specs and RFCs
- System design docs
- Runbooks
- Postmortems
- Brainstorms and working notes

The output is a single portable `.html` file. Styling and scripts are embedded. Mermaid and fonts are loaded from CDNs when the file is opened.

## Design

- Dark blue theme by default
- Light mode toggle
- Inter for body text
- JetBrains Mono for code
- Sticky desktop contents sidebar
- Mobile contents drawer
- Scroll progress
- Heading permalinks
- Copy buttons on code blocks
- WCAG-conscious contrast, focus states, and reduced-motion handling

## Install

### Claude Code

```bash
git clone https://github.com/miquael/MarkUp ~/.claude/skills/markup
```

Reload your session and run:

```bash
/markup plan.md
```

Outputs `plan.html` next to the source.

### Codex CLI

```bash
git clone https://github.com/miquael/MarkUp ~/.config/markup
mkdir -p ~/.codex/prompts
ln -s ~/.config/markup/SKILL.md ~/.codex/prompts/markup.md
```

Codex's prompts directory has shifted across versions; adjust the symlink target if needed.

## Usage

```bash
/markup plan.md
/markup plan.md --out docs/plan.html
/markup
```

The output is written next to the source unless `--out` is provided.

## Components

| Source pattern | Output component |
|---|---|
| Ordered rollout or action list | Timeline |
| Architecture flow | Mermaid diagram |
| Pros and cons | Pros / Cons panel |
| Option comparison | Comparison cards |
| Numeric targets or outcomes | Metric grid |
| Criteria-based choice | Decision matrix |
| Risks and mitigations | Risk register |
| Draft, accepted, blocked, deprecated state | Status badges |
| Critical conclusion | Key-point highlight |
| Warnings, decisions, tips | Callouts |
| Long appendix or FAQ | Collapsible section |
| Wide data | Responsive table wrapper |

## Examples

See:

- [`examples/example-plan.md`](examples/example-plan.md) -> [`examples/example-plan.html`](examples/example-plan.html)
- [`examples/technical-spec.md`](examples/technical-spec.md) -> [`examples/technical-spec.html`](examples/technical-spec.html)
- [`examples/architecture-rfc.md`](examples/architecture-rfc.md) -> [`examples/architecture-rfc.html`](examples/architecture-rfc.html)
- [`examples/incident-postmortem.md`](examples/incident-postmortem.md) -> [`examples/incident-postmortem.html`](examples/incident-postmortem.html)
- [`examples/component-showcase.md`](examples/component-showcase.md) -> [`examples/component-showcase.html`](examples/component-showcase.html)

`example-plan` is the default compact reference pair. The other examples are optional references for technical specs, RFCs, postmortems, and component-heavy layouts.

## Project Files

```text
MarkUp/
|-- SKILL.md
|-- template.html
|-- components.md
|-- examples/
|-- docs/
|-- LICENSE
`-- README.md
```

## Customization

The template is intentionally portable. Keep generated HTML self-contained unless you have a specific reason to link shared assets.

Theme tokens live at the top of `template.html`:

```css
:root {
  --accent: #2563EB;
  --bg: #F7FAFC;
}

[data-theme="dark"] {
  --accent: #60A5FA;
  --bg: #07111F;
}
```

## Roadmap

- Add optional offline Mermaid bundling
- Add richer table variants

## License

MIT.

## Attribution

[MarkUp](https://github.com/miquael/MarkUp) is extended from [haidang1810/md2html](https://github.com/haidang1810/md2html).
