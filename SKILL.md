---
name: markup
description: Convert long-form English Markdown into a single portable MarkUp HTML page with Mermaid diagrams, timelines, callouts, comparison cards, metrics, decision matrices, risk registers, sidebar contents, and a blue dark-default theme.
trigger: /markup
---

# /markup

Convert a verbose English Markdown document into a single, self-contained HTML file that is easy to scan: diagrams instead of dense flow prose, step cards instead of rollout lists, callouts for the parts that matter, and structured components for metrics, decisions, and risks.

## Usage

```
/markup <file.md>             # output <file>.html next to source
/markup <file.md> --out X.html # custom output path
/markup                       # if no arg, ask user which file
```

## Skill files

Resolve these files relative to this `SKILL.md`:

- `template.html` - HTML skeleton with embedded CSS, Mermaid CDN, theme toggle, sidebar contents, footer, placeholders, and content slots.
- `components.md` - HTML component catalog for timelines, callouts, diagrams, pros/cons, comparison cards, metrics, decision matrices, risk registers, collapsibles, tables, images, task lists, and footnotes.
- `examples/example-plan.md` and `examples/example-plan.html` - default reference pair. Read this pair before converting.
- `examples/technical-spec.md` and `examples/technical-spec.html` - optional reference for specifications, steering docs, implementation notes, and research-to-build reports.
- `examples/architecture-rfc.md` and `examples/architecture-rfc.html` - optional reference for architecture proposals and trade-off discussions.
- `examples/incident-postmortem.md` and `examples/incident-postmortem.html` - optional reference for incidents, timelines, and corrective actions.
- `examples/component-showcase.md` and `examples/component-showcase.html` - optional visual reference for component-heavy layouts.

Read `template.html`, `components.md`, and the default example pair before writing output. Do not invent CSS classes or skip the catalog.

## Conversion Steps

### Step 1 - Resolve Inputs

1. Determine the source file from the invocation. If none is provided, ask: "Which `.md` file should I convert?" and stop.
2. Read the source `.md` fully.
3. Read `template.html` and `components.md`.
4. Read `examples/example-plan.md` and `examples/example-plan.html` to calibrate output quality. If the source resembles a technical spec, RFC, postmortem, or component-heavy showcase, optionally read the relevant additional example.

### Step 2 - Analyze The Source

Identify:

- **Title** - first H1, or the filename. Keep it at or below 80 characters when possible.
- **Subtitle** - first paragraph after the H1, or the document's clearest summary sentence. Keep it at or below 200 characters.
- **Doc type** - infer one of `PLAN`, `SPEC`, `SYSTEM DESIGN`, `RFC`, `RUNBOOK`, `POSTMORTEM`, `BRAINSTORM`, `NOTES`.
- **Brand label** - title-case version of the doc type, such as `Plan`, `Spec`, `System Design`, `RFC`, `Runbook`, `Postmortem`, `Brainstorm`, or `Notes`.
- **Reading time** - words divided by 250, rounded to the nearest minute. Format as `~N min read`.
- **Section map** - walk each H2/H3 and pick the best component from `components.md`.

Use these component heuristics:

| Source pattern | Component |
|---|---|
| Ordered rollout, action items, migration steps | Timeline |
| Architecture flow with three or more hops | Mermaid diagram |
| Pros, cons, trade-offs | Pros / Cons |
| Option A vs B vs C | Comparison cards |
| Numeric targets or impact summary | Metric grid |
| Weighted criteria or scoring | Decision matrix |
| Risks, likelihood, impact, mitigation | Risk register |
| Draft, accepted, blocked, deprecated, rollout state | Status badges |
| Critical conclusion | Key-point highlight |
| Context, note, background | Info callout |
| Risk, gotcha, caution | Warning callout |
| Blocker, destructive action, must-not-do | Danger callout |
| Completed work or verified result | Success callout |
| Chosen path or ADR | Decision callout |
| Recommendation or best practice | Tip callout |
| Long appendix, FAQ, rejected options | Collapsible |
| Four or more columns or long cells | Table wrapper |

### Step 3 - Build The Output HTML

1. Copy the full `template.html` content into an output buffer.
2. Replace placeholders:
   - `{{REC_LABEL}}` -> `Recommended`
   - `{{TITLE}}`
   - `{{SUBTITLE}}`
   - `{{DOC_TYPE}}`
   - `{{SOURCE_FILE}}`
   - `{{DATE}}` -> ISO date or `Updated <date>`
   - `{{READ_TIME}}`
   - `{{BRAND_LABEL}}`
   - `{{TOC_TITLE}}` -> `Contents`
   - `{{PRINT_TOOLTIP}}` -> `Print / Save PDF`
   - `{{THEME_TOOLTIP}}` -> `Toggle theme`
   - `{{CLOSE_LABEL}}` -> `Close`
   - `{{SKIP_LINK_LABEL}}` -> `Skip to content`
   - `{{FOOTER_NOTE}}` -> `Source: <source basename>`
3. Replace `<!-- TOC_ENTRIES -->` with one link per H2/H3.
4. Replace the slot between `<!-- CONTENT_START -->` and `<!-- CONTENT_END -->` with the document body.
5. Write the assembled HTML to the output path.

Each section should:

- Start with `<h2 id="...">` matching the TOC entry.
- Use one primary component per logical chunk.
- Preserve technical detail and exact identifiers.
- Avoid adding information not present in the source.

### Step 4 - Verify

After writing, run a quick sanity check:

- Every TOC `href` points to an existing heading `id`.
- No leftover `{{PLACEHOLDER}}` strings.
- Mermaid blocks use valid forms such as `flowchart`, `sequenceDiagram`, `erDiagram`, `stateDiagram-v2`, or `gantt`.
- No `<script>` tags were added beyond the scripts already in `template.html`.
- The document is English-only unless the source contains proper nouns, code identifiers, filenames, commands, or URLs.

Report:

- Output file path.
- One-line rendering summary, such as: "Rendered 8 sections: 1 Mermaid flow, 1 timeline, 2 callouts, 1 risk register. ~6 min read."
- A reminder that the user can open it with `open <file>.html` on macOS or `xdg-open <file>.html` on Linux.

## Critical Rules

1. Preserve exact technical content. Do not turn `0042_user_schema.sql` into `the new migration`.
2. Keep components flat. Do not nest callouts inside timelines inside collapsibles.
3. Use Mermaid for any flow with three or more meaningful hops.
4. Use key-point highlights sparingly.
5. UI is English only.
6. Output is portable. Do not add external file references beyond the CDN scripts already in `template.html`.
7. Do not modify `template.html` or `components.md` while converting a user document. Those files are MarkUp's source of truth.
8. Use SVG icons from the template sprite. Do not use emoji icons.
9. Anchor links and copy-to-clipboard buttons are injected by the template script. Do not add them manually.
10. Wrap wide tables in `.table-wrap`.
11. Use `<figure>` and `<figcaption>` for images with descriptive `alt` text.

## Cross-Agent Compatibility

MarkUp is designed to work in any AI agent that can read and write local files:

- Claude Code - install this directory as a skill and invoke `/markup <file>`.
- Codex CLI - make this prompt available to Codex and keep `template.html` and `components.md` at a stable path.
- Other agents - paste this file into the agent instructions and grant access to this directory.

The only external dependency is Mermaid via CDN at HTML open time. Fonts are loaded from Google Fonts. No package install is required for generation.

## Edge Cases

- **Source has no headings** - create one `<h2 id="content">Content</h2>` and infer logical breaks.
- **Source has existing Mermaid blocks** - keep them and wrap in `<figure class="diagram">`.
- **Source has embedded HTML** - pass through if safe, otherwise escape.
- **Source is very short** - skip the sidebar contents and render a single column if the TOC would be empty.
- **Source is very long** - collapse low-priority appendices with `<details>`.
- **Output file already exists** - overwrite it. The Markdown source is canonical.

## Anti-Patterns

- Generating HTML through many small edits.
- Adding generated-only CSS via `<style>`.
- Translating, rewriting, or smoothing away technical identifiers.
- Adding facts not present in the source.
- Reporting success without verification.
