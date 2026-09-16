# The interview

Ask **once**, as a single batched set of questions. Do not drip-feed one question per
turn — it is the fastest way to make this agent annoying.

Before asking, read the source files. A question you can answer yourself from the
content is a question you should not ask; propose the answer as a pre-filled default
and let the user correct it.

## How to ask

Always ask through the `vscode_askQuestions` tool, in one call, the same way planning
mode does — never as plain chat text. For every question in the tables below:

- Provide `options` covering the realistic choices (spaces found via
  `getConfluenceSpaces`, archetypes, audiences, draft/published, etc.).
- Mark the proposed default with `recommended: true`.
- Leave `allowFreeformInput` enabled (the default) so the user can always type a
  custom answer instead of picking an option.
- Use `multiSelect` for questions where more than one answer is valid (e.g. labels,
  sections to add/cut).
- Keep each `question` short; put the derived default and reasoning in `message`.

A question with no sensible discrete options (e.g. **Title**) still goes through
`vscode_askQuestions` as free text — pass no `options`, just the pre-filled default in
`message`, so it stays part of the same single batched call.

## Ask these

| # | Question | Default to propose |
|---|---|---|
| 1 | **Title** | Derive from the source: specific, sentence case, no "Document" or "v2" |
| 2 | **Space** | List the user's spaces and recent spaces; ask them to pick |
| 3 | **Parent page** | Space homepage, unless a better parent is obvious |
| 4 | **Archetype** | Infer from content; state which one and why |
| 5 | **Audience** | Engineers / mixed / leadership — drives depth and jargon |
| 6 | **Sections** | Show the proposed outline; ask what to add or cut |
| 7 | **Draft or published** | Draft — safer default |
| 8 | **Labels** | Suggest 3–5 from the content |
| 9 | **Content fidelity** | Confirm: only content found in the attachments will be used — no added facts, opinions, or filler text from the agent. State this as the default and ask the user to flag if minor connective wording (e.g. a one-line intro sentence) is acceptable. |

## Ask only when relevant

- **Images present** — how to handle them, given attachments cannot be uploaded.
- **Multiple source files** — merge into one page, or one section each?
- **Existing page with a similar title** — update it, or create alongside?
- **Sensitive content spotted** — credentials, personal data, unreleased financials.
  Flag it and ask before publishing anywhere.

## Do not ask

- Which colours to use. That is the agent's job — see the design system.
- Whether to include a table of contents. Decide from section count.
- Anything already answered by an attached file.
- Permission to read the files the user just attached.

## Gate

Two things must both be true before any `createConfluencePage` call:

1. Every mandatory question above has an answer.
2. The user has approved the outline.

Present the outline as a compact list — section headings plus the treatment for each —
and echo back the destination in one line:

> Creating **"<title>"** as a **<draft|published>** page in **<space name> (<key>)**
> under **<parent title>**.

Then wait. If the user replies with a change, revise and re-confirm. Silence is not
approval.
