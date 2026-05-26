# MarkUp Component Catalog

This file is the source of truth for the HTML snippets used when filling `template.html`.

Rules:
- Copy snippets verbatim, only replace `{{...}}` placeholders.
- Do not invent CSS classes. Use this catalog or plain HTML (`<h2>`, `<p>`, `<ul>`, `<table>`, etc.).
- Output UI is English only.
- Body content should stay faithful to the source. Do not add facts that are not in the source.
- Use SVG icons via the sprite. Do not use emoji icons.

## 1. Page Metadata

Replace the placeholders in `template.html`:

| Placeholder | Example |
|---|---|
| `{{REC_LABEL}}` | Recommended |
| `{{TITLE}}` | API Migration Plan |
| `{{SUBTITLE}}` | A phased rollout for replacing the legacy endpoint. |
| `{{DOC_TYPE}}` | PLAN, SPEC, SYSTEM DESIGN, RFC, RUNBOOK, POSTMORTEM, BRAINSTORM, NOTES |
| `{{SOURCE_FILE}}` | migration-plan.md |
| `{{DATE}}` | Updated 2026-05-26 |
| `{{READ_TIME}}` | ~5 min read |
| `{{BRAND_LABEL}}` | Plan, Spec, System Design, RFC, Runbook, Postmortem, Brainstorm, Notes |
| `{{TOC_TITLE}}` | Contents |
| `{{PRINT_TOOLTIP}}` | Print / Save PDF |
| `{{THEME_TOOLTIP}}` | Toggle theme |
| `{{CLOSE_LABEL}}` | Close |
| `{{SKIP_LINK_LABEL}}` | Skip to content |
| `{{FOOTER_NOTE}}` | Source: migration-plan.md |

The `<html>` element is always `lang="en"`. Do not translate UI labels.

## 2. TOC Entry

Goes inside `<nav class="toc-nav">`.

```html
<a href="#section-id" class="lvl-2">Section title</a>
<a href="#subsection-id" class="lvl-3">Subsection title</a>
```

Use one entry per H2/H3. The `href` must match the heading `id`.

## 3. Step Card / Timeline

Use for action plans, rollout steps, migrations, workflows, and incident timelines.

```html
<div class="timeline">
  <article class="step">
    <div class="step-num">1</div>
    <div class="step-body">
      <h3>Prepare the schema</h3>
      <p>Create the new table, backfill existing records, and verify row counts.</p>
      <div class="step-tags">
        <span class="tag">database</span>
        <span class="tag">2 days</span>
      </div>
    </div>
  </article>
  <article class="step done">
    <div class="step-num">2</div>
    <div class="step-body">
      <h3>Ship read path</h3>
      <p>Enable the new read model behind a feature flag for internal users.</p>
    </div>
  </article>
</div>
```

## 4. Callouts

Use callouts for important context, warnings, decisions, confirmations, and tips.

### Info

```html
<aside class="callout callout-info">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-info"/></svg>
  <div class="callout-body">
    <p class="callout-title">Context</p>
    <p>The current worker scans the database every five minutes, which creates avoidable latency.</p>
  </div>
</aside>
```

### Warning

```html
<aside class="callout callout-warn">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-warn"/></svg>
  <div class="callout-body">
    <p class="callout-title">Heads up</p>
    <p>The migration can lock the orders table for up to 30 seconds during peak traffic.</p>
  </div>
</aside>
```

### Danger

```html
<aside class="callout callout-danger">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-danger"/></svg>
  <div class="callout-body">
    <p class="callout-title">Do not do this</p>
    <p>Do not drop the legacy column until all production workers run the new release.</p>
  </div>
</aside>
```

### Success

```html
<aside class="callout callout-success">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-success"/></svg>
  <div class="callout-body">
    <p class="callout-title">Done</p>
    <p>The load test passed at 10k requests per second with p99 latency under 90ms.</p>
  </div>
</aside>
```

### Decision

```html
<aside class="callout callout-decision">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-decision"/></svg>
  <div class="callout-body">
    <p class="callout-title">Decision</p>
    <p>Use Postgres because the workflow needs transactional consistency.</p>
  </div>
</aside>
```

### Tip

```html
<aside class="callout callout-tip">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-tip"/></svg>
  <div class="callout-body">
    <p class="callout-title">Tip</p>
    <p>Cache this query for five minutes to reduce database load during bursts.</p>
  </div>
</aside>
```

### Security

```html
<aside class="callout callout-info">
  <svg class="callout-icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-lock"/></svg>
  <div class="callout-body">
    <p class="callout-title">Security</p>
    <p>The webhook stays inside the private network and never crosses the public internet.</p>
  </div>
</aside>
```

## 5. Status Badges

Use for document state, feature state, rollout state, and lightweight labels.

```html
<div class="status-row">
  <span class="status-badge draft">Draft</span>
  <span class="status-badge proposed">Proposed</span>
  <span class="status-badge accepted">Accepted</span>
  <span class="status-badge blocked">Blocked</span>
  <span class="status-badge deprecated">Deprecated</span>
</div>
```

Use only short labels. For explanatory text, use a callout or paragraph.

## 6. Key-Point Highlight

Use sparingly for the most important conclusion in a section.

```html
<div class="highlight">
  <span class="highlight-label">Key point</span>
  <p>Moving from polling to event delivery cuts latency from minutes to seconds while reducing database load.</p>
</div>
```

## 7. Mermaid Diagram

Use diagrams for flows with three or more hops, state machines, entity relationships, sequence diagrams, or roadmaps.

```html
<figure class="diagram">
  <div class="diagram-toolbar" aria-label="Diagram controls">
    <button class="icon-btn" type="button" data-diagram-action="zoom-in" title="Zoom in" aria-label="Zoom in">
      <svg class="icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-zoom-in"/></svg>
    </button>
    <button class="icon-btn" type="button" data-diagram-action="zoom-out" title="Zoom out" aria-label="Zoom out">
      <svg class="icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-zoom-out"/></svg>
    </button>
    <button class="icon-btn" type="button" data-diagram-action="reset" title="Reset" aria-label="Reset">
      <svg class="icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-reset"/></svg>
    </button>
    <button class="icon-btn" type="button" data-diagram-action="expand" title="Expand" aria-label="Expand">
      <svg class="icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-expand"/></svg>
    </button>
  </div>
  <div class="diagram-viewport">
    <div class="diagram-canvas" data-scale="1">
      <pre class="mermaid">
flowchart LR
  Client --> Gateway
  Gateway --> Worker
  Worker --> DB[(Postgres)]
  Worker --> Queue[/Event bus/]
      </pre>
    </div>
  </div>
  <figcaption class="diagram-caption">Request flow from client to worker and persistence layers.</figcaption>
</figure>
```

Prefer `flowchart LR`, `flowchart TD`, `sequenceDiagram`, `erDiagram`, `stateDiagram-v2`, or `gantt`.

Use this full wrapper for every Mermaid diagram. The toolbar lets readers zoom, reset, and expand dense diagrams without changing the source Mermaid.

## 8. Pros / Cons

Use for trade-off sections.

```html
<div class="proscons">
  <div class="proscons-col pros">
    <h4>Pros</h4>
    <ul>
      <li>Fast to ship and easy to roll back.</li>
      <li>Compatible with the current clients.</li>
    </ul>
  </div>
  <div class="proscons-col cons">
    <h4>Cons</h4>
    <ul>
      <li>Adds routing complexity.</li>
      <li>Maintains two paths during the rollout window.</li>
    </ul>
  </div>
</div>
```

## 9. Comparison Cards

Use for two or more options.

```html
<div class="compare">
  <div class="compare-card recommended">
    <h4>Option B - Hybrid cache</h4>
    <p>Combines Redis for speed with database fallback for correctness.</p>
  </div>
  <div class="compare-card">
    <h4>Option A - Database only</h4>
    <p>Simplest, but peak latency remains too high.</p>
  </div>
  <div class="compare-card">
    <h4>Option C - In-memory map</h4>
    <p>Fastest, but loses state on restart and does not scale horizontally.</p>
  </div>
</div>
```

Put the recommended option first.

## 10. Metric Grid

Use for measurable targets, baselines, outcomes, and postmortem impact summaries.

```html
<div class="metric-grid">
  <div class="metric-card">
    <span class="metric-label">Latency</span>
    <span class="metric-value">4.8s</span>
    <p class="metric-note">Target p99 after rollout.</p>
  </div>
  <div class="metric-card">
    <span class="metric-label">DB load</span>
    <span class="metric-value">-60%</span>
    <p class="metric-note">Expected reduction from removing polling.</p>
  </div>
  <div class="metric-card">
    <span class="metric-label">Rollout</span>
    <span class="metric-value">5 weeks</span>
    <p class="metric-note">Includes monitoring and cleanup.</p>
  </div>
</div>
```

## 11. Decision Matrix

Use when criteria matter more than prose comparison.

```html
<div class="decision-matrix">
  <table>
    <thead>
      <tr>
        <th>Criterion</th>
        <th>Kafka</th>
        <th>SQS</th>
        <th>RabbitMQ</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Replay support</td>
        <td class="winner">Strong</td>
        <td>Limited</td>
        <td>Weak</td>
      </tr>
      <tr>
        <td>Operational cost</td>
        <td>Existing cluster</td>
        <td>Managed service fee</td>
        <td>New operations burden</td>
      </tr>
      <tr>
        <td>Total score</td>
        <td class="score winner">9/10</td>
        <td class="score">7/10</td>
        <td class="score">5/10</td>
      </tr>
    </tbody>
  </table>
</div>
```

## 12. Risk Register

Use for risks, likelihood, impact, and mitigation.

```html
<div class="risk-register">
  <article class="risk-item">
    <div>
      <h4>Duplicate notifications during double-write</h4>
      <p>Mitigation: require idempotency keys on every event before enabling producer writes.</p>
    </div>
    <div class="risk-meta">
      <span class="risk-pill medium">Likelihood: medium</span>
      <span class="risk-pill high">Impact: high</span>
    </div>
  </article>
  <article class="risk-item">
    <div>
      <h4>Consumer lag during peak traffic</h4>
      <p>Mitigation: scale partitions and alert when lag exceeds the rollback threshold.</p>
    </div>
    <div class="risk-meta">
      <span class="risk-pill low">Likelihood: low</span>
      <span class="risk-pill medium">Impact: medium</span>
    </div>
  </article>
</div>
```

## 13. Collapsible Section

Use for optional deep dives, appendices, long code blocks, rejected options, or FAQ content.

```html
<details class="collapsible">
  <summary>Retry policy details</summary>
  <div class="collapsible-body">
    <p>Retries use exponential backoff with jitter and send exhausted events to the dead-letter queue.</p>
    <pre><code>retryDelays = [1, 4, 16, 64, 256, 1024]</code></pre>
  </div>
</details>
```

## 14. Plain HTML Elements

These render correctly with the template CSS:

| Markdown source | HTML to write |
|---|---|
| `## Heading` | `<h2 id="heading">Heading</h2>` |
| `### Subheading` | `<h3 id="subheading">Subheading</h3>` |
| `**bold**` | `<strong>bold</strong>` |
| `` `code` `` | `<code>code</code>` |
| code fence | `<pre><code>code</code></pre>` |
| bullet list | `<ul><li>Item</li></ul>` |
| numbered list | `<ol><li>Item</li></ol>` |
| quote | `<blockquote>Quote</blockquote>` |
| table | `<table>...</table>` |
| link | `<a href="url">text</a>` |

Do not use `<h1>` in body content. The template title is already the page H1.

## 15. Component Selection Cheatsheet

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

## 16. Icon Sprite Catalog

`template.html` defines these icon IDs. Reference them as:

```html
<svg class="icon" viewBox="0 0 24 24" aria-hidden="true"><use href="#i-info"/></svg>
```

| Icon ID | Use |
|---|---|
| `#i-info` | Info callout |
| `#i-warn` | Warning callout |
| `#i-danger` | Danger callout |
| `#i-success` | Success callout |
| `#i-decision` | Decision callout |
| `#i-tip` | Tip callout |
| `#i-lock` | Security note |
| `#i-printer` | Print button |
| `#i-moon` | Dark theme icon |
| `#i-sun` | Light theme icon |
| `#i-menu` | Mobile contents button |
| `#i-x` | Close button |
| `#i-file` | Source file metadata |
| `#i-calendar` | Date metadata |
| `#i-clock` | Read-time metadata |
| `#i-copy` | Copy button |
| `#i-check` | Copied state |
| `#i-link` | Heading permalink |

## 17. Edge-Case Patterns

### Images

```html
<figure>
  <img src="path/to/image.png" alt="Architecture diagram for the notification service">
  <figcaption>Notification service deployment topology.</figcaption>
</figure>
```

Use descriptive `alt` text. If the image is decorative, use `alt=""`.

### Wide Tables

```html
<div class="table-wrap">
  <table>
    <thead>
      <tr><th>Service</th><th>Owner</th><th>SLO</th><th>Risk</th></tr>
    </thead>
    <tbody>
      <tr><td>API</td><td>Platform</td><td>99.9%</td><td>Medium</td></tr>
    </tbody>
  </table>
</div>
```

Wrap tables with four or more columns, or tables with long cells.

### Task Lists

```html
<ul>
  <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled> Add alerts</li>
  <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled checked> Create dashboard</li>
</ul>
```

### Footnotes

```html
<p>This behavior matches the current retry policy<sup class="footnote-ref"><a href="#fn1" id="fnref1">1</a></sup>.</p>

<ol class="footnotes">
  <li id="fn1">Retry policy version 2. <a href="#fnref1">Back</a></li>
</ol>
```

## 18. Anti-Patterns

- Do not use emoji icons.
- Do not wrap every paragraph in callouts.
- Do not use timelines for ordinary numbered lists.
- Do not make Mermaid diagrams too large. Split diagrams above roughly 15 nodes.
- Do not inline new styles in generated content.
- Do not add scripts beyond the template.
- Do not add facts that are not in the source.
- Do not translate UI or body content into another language.
