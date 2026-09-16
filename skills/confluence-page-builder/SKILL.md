---
name: confluence-page-builder
description: 'Turn files into a polished Confluence page. Use when the user wants to create, publish, generate, or write a Confluence page, Confluence doc, or wiki page from documents — PDFs, Word docx, PowerPoint pptx, markdown, SQL, code, CSV, or images. Runs a clarifying interview about title, space, parent page, and outline before creating anything.'
argument-hint: 'Attach the source files, e.g. /confluence-page-builder #file:report.md #file:deck.pptx'
---

# Confluence page builder

Converts source files into a well-designed Confluence page via the Atlassian MCP
server. Interview first, publish last.

## Hard rules

- **Never** call `createConfluencePage` or `updateConfluencePage` before the interview
  is answered *and* the user has approved the outline.
- **Never** author Confluence storage-format XML. `<ac:structured-macro>`,
  `<ac:rich-text-body>`, `<ri:page>` and CDATA bodies render as visible raw text.
  Get the correct markup from `getContentFormatGuide` every run.
- **Never** update or overwrite an existing page without explicit confirmation naming
  that page. Never change a space, its settings, permissions, or templates.
- **Never** delete or archive anything, with one exception: a page you created earlier
  in this same conversation, when the user explicitly asks you to remove it.
- **Never** invent facts absent from the sources. If a section would need invention,
  drop the section and say so.
- **Never** add your own commentary, opinions, filler explanations, or content not
  present in the attachments — confirm this constraint explicitly in the interview
  (see [interview.md](./references/interview.md)) rather than assuming it.

## Procedure

### 1. Read the sources

Resolve every attached or referenced file.

| Source | How to read it |
|---|---|
| `.md` `.txt` `.sql` `.csv` `.json` code | `read_file` directly |
| `.pdf` `.docx` `.pptx` `.xlsx` `.html` | `mcp_markitdown_convert_to_markdown` |
| `.png` `.jpg` `.gif` `.webp` | `view_image` — describe it, you cannot upload it |

markitdown takes a URI, not a path. Convert Windows paths to
`file:///C:/...` with forward slashes and spaces percent-encoded as `%20`.
Apostrophes and `@` are fine literally. Example:

```
file:///C:/Users/me/OneDrive%20-%20Contoso/Docs/deck.pptx
```

Large results land in a temp file — read it with `read_file`.

For a slide deck, read the speaker notes too, and see the "slide decks" note in
[layouts.md](./references/layouts.md) before mirroring its structure.

### 2. Interview

Follow [interview.md](./references/interview.md). One batched set of questions with
pre-filled defaults derived from step 1, asked through `vscode_askQuestions` —
multiple-choice options with a recommended default, same as planning mode, always
leaving room for a custom freeform answer.

Use `getAccessibleAtlassianResources` for the `cloudId`, then `getConfluenceSpaces`
to offer real space choices, and `getPagesInConfluenceSpace` to resolve the parent.
Run `searchConfluenceUsingCql` for a page with a similar title — if one exists, say so
before creating a duplicate.

### 3. Get the outline approved

Present section headings plus the treatment for each, and echo the destination back in
one line. Wait for a reply. Silence is not approval.

### 4. Compose

Call `getContentFormatGuide` with `toolName: "createConfluencePage"` and follow it
exactly — it is the authoritative markup spec and it changes independently of this
skill. Then apply [design-system.md](./references/design-system.md) for colour
semantics, page anatomy, and density, and the chosen archetype from
[layouts.md](./references/layouts.md) for section structure.

Make the page as interactive and scannable as the format guide allows — panels,
status lozenges, expands, tables of contents, multi-column layouts, task lists — see
"Interactive elements" in [design-system.md](./references/design-system.md). Every
element must still be built only from what the sources contain; never invent content
to justify using an element.

Leave `contentFormat` at its default (`html`). Only drop to `markdown` if the guide
call fails, and tell the user the page will be plainer as a result.

### 5. Publish and report

Create the page, then report:

- The page URL, as a clickable link.
- Space, parent, and draft/published status.
- Every image placeholder the user must add manually, with its location on the page.
- Anything from the sources you deliberately left out, and why.

If creation fails, do not retry blindly. Report the error, state the likely cause
(missing scope, bad `spaceId`, malformed body), and propose a fix.

## Known constraints

- The MCP token has **no attachment scope** — images cannot be uploaded with the page.
  Handle this per the design system rather than silently dropping them.
- `spaceId` accepts a numeric ID or a space key; personal-space keys look like `~712020abc`.
- Labels are not settable through the create call. If the user wants labels, tell them
  to add them in the UI rather than pretending they were applied.
