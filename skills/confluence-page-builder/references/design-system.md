# Design system

How to make a Confluence page that people actually want to read. This file covers
**design decisions**. It deliberately does **not** specify markup.

> **Markup comes from `getContentFormatGuide`, never from here.**
> `createConfluencePage` accepts a custom ADF-mapped HTML that is *not* Confluence
> storage format. Storage-format XML (`<ac:structured-macro>`, `<ac:rich-text-body>`,
> `<ri:page>`, CDATA bodies) renders as visible raw text and ruins the page. Call
> `getContentFormatGuide` with `toolName: "createConfluencePage"` and copy its
> patterns exactly. If the guide and this file ever disagree, the guide wins.

## The five rules

1. **Answer first.** The reader gets the conclusion in the first screen — before any
   background, methodology, or table of contents.
2. **Every screen needs one non-paragraph element.** A panel, table, status lozenge,
   divider, or expand. Walls of prose are the failure mode we are designing against.
3. **Colour carries meaning, never decoration.** See the semantic table below. A page
   where colour is decorative is harder to scan than a page with no colour at all.
4. **Detail goes in expands.** Long SQL, raw logs, full data dumps, appendices. The
   main flow stays skimmable; the depth is one click away.
5. **Three panels per screen is the ceiling.** Past that, panels stop signalling
   importance and become noise.

## Colour semantics

Pick the panel type by meaning, not by which colour looks nice.

| Meaning | Panel | Use for |
|---|---|---|
| Neutral context | info (blue) | Scope, assumptions, definitions, "how to read this page" |
| Secondary aside | note (purple) | Tangents, historical context, related-work pointers |
| Confirmed / resolved | success (green) | Verified findings, completed migrations, passing checks |
| Needs attention | warning (yellow) | Caveats, known gaps, data-quality limitations, deadlines |
| Danger / blocking | error (red) | Breaking changes, data loss risk, production incidents |

Status lozenges follow the same logic: green = done/healthy, yellow = in progress,
red = blocked/failed, blue = informational, purple = planned, grey = not started.

Reserve red. A page with three red panels has taught the reader that red means nothing.

## Page anatomy

Sections in this order. Skip any that the source material does not support — an empty
section is worse than a missing one.

1. **Summary panel** — 2–4 sentences. What this is, who it is for, the headline
   conclusion. Info panel unless the news is bad.
2. **Status strip** — a short row of lozenges: owner, state, date, version. Optional
   but cheap and it makes the page feel maintained.
3. **Table of contents** — only when the page exceeds roughly six sections. On a short
   page a TOC is pure overhead.
4. **Body sections** — driven by the archetype. See [layouts.md](./layouts.md).
5. **Expands** — supporting detail, raw queries, appendices.
6. **Next steps / owners** — an action table, or a task list if the actions are
   genuinely trackable.

## Interactive elements

Use as many of Confluence's native interactive/structural elements as the source
content supports and the format guide allows — they are what make a page feel
maintained rather than pasted. Never add one to fill space; only build them from
content that exists in the sources.

| Element | Use for |
|---|---|
| Panel (info/note/success/warning/error) | Callouts per the colour semantics table above |
| Status lozenge | Owner, state, priority, version at a glance |
| Expand | Long SQL, logs, raw data, appendices — keeps the main flow skimmable |
| Table of contents | Navigation on pages past ~6 sections |
| Multi-column layout | Side-by-side metric cards, before/after comparisons |
| Table | Any structured, comparable, or repeating data |
| Task list | Genuinely trackable next steps or owners |
| Divider | Separating major sections without adding a heading |

Still obey the ceilings above: three panels per screen, no TOC on a short page, no
table for a single row. More interactivity is only better when the underlying content
justifies it.

## Tables

- Header row always on.
- Lead with the column the reader scans for; put prose in the last column.
- Put a status lozenge in its own narrow column rather than colouring text.
- Beyond ~8 columns, split into two tables or move detail into an expand.
- Never use a table purely for layout. Use a layout section for that.

## Multi-column layouts

Two-column earns its place for: metric cards side by side, before/after comparisons,
a summary paired with a diagram. It costs you on mobile, so never put a wide table or
a code block inside a column. Three columns only for three short metric cards.

## Code

Always a real code block with the language set — never indented paragraphs, never a
table cell. Anything past ~30 lines belongs in an expand with a one-line description
of what the reader would learn by opening it.

## Images

The current MCP token has **no attachment scope**, so images cannot be uploaded with
the page. Handle it explicitly rather than silently dropping them:

- If the image is already at a public/intranet https URL, reference the URL.
- Otherwise, describe the image in a note panel where it should appear, tell the user
  which file to drag in and where, and list every such placeholder in the final report.
- Never silently omit an image the user supplied.

## Voice

Present tense, active voice, second person for instructions. Expand every acronym on
first use. Cut hedging ("it seems that", "arguably"). Headings state the finding
("Stock counts drift after midnight resyncs") rather than naming the topic
("Analysis"). Sentence case throughout.

## Failure modes

- Storage-format XML in the body → renders as raw text. The single worst outcome.
- Rainbow pages where every section has a differently coloured panel.
- A TOC on a three-section page.
- Restating the summary as the first body section.
- Dumping an entire source document into one expand and calling it structure.
- Tables with a single row — that is a panel.
