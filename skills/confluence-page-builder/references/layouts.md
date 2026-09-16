# Page archetypes

Pick one archetype during the interview and follow its section list. The archetype is
the single biggest driver of whether the page feels purposeful or generic.

If the source material genuinely does not fit any of these, build a custom outline —
but still get it approved before composing.

---

## 1. Analysis / root-cause report

For investigations, post-incident writeups, data-quality findings.
Typical sources: a markdown writeup, SQL queries, query results.

| # | Section | Treatment |
|---|---|---|
| 1 | Summary | Info panel — or warning/error if the finding is bad news |
| 2 | Status strip | Severity, owner, investigation date |
| 3 | What we observed | Prose plus an evidence table |
| 4 | Root causes | One subheading per cause, each with an impact lozenge |
| 5 | Contributing factors | Bullets, or a table if there are more than five |
| 6 | Impact | Table: area / magnitude / confidence |
| 7 | Recommendations | Table: action / owner / priority lozenge |
| 8 | Appendix — queries | Expand per query, code block inside |

Lead with the conclusion. The narrative of how you got there goes in section 3, not
section 1.

---

## 2. Technical design / architecture

For proposals, design docs, ADR-adjacent pages.
Typical sources: a design doc, diagrams, schema files.

| # | Section | Treatment |
|---|---|---|
| 1 | Summary | Info panel |
| 2 | Status strip | Status, author, decision date, reviewers |
| 3 | Context and problem | Prose |
| 4 | Goals / non-goals | Two-column layout section |
| 5 | Proposed design | Prose plus diagram (see image handling) |
| 6 | Alternatives considered | Table: option / pros / cons / verdict lozenge |
| 7 | Risks | Warning panel or a risk table |
| 8 | Open questions | Task list |
| 9 | Appendix | Expands for schemas and interface definitions |

Goals/non-goals side by side is the highest-value layout choice on this archetype.

---

## 3. Runbook / how-to

For operational procedures and onboarding guides.
Typical sources: a procedure doc, scripts, screenshots.

| # | Section | Treatment |
|---|---|---|
| 1 | When to use this | Info panel |
| 2 | Prerequisites | Checklist or task list |
| 3 | Safety warnings | Warning or error panel — place *before* the steps |
| 4 | Procedure | Numbered list; code block per command |
| 5 | Verification | Table: check / expected result |
| 6 | Rollback | Expand, or its own section if rollback is likely |
| 7 | Troubleshooting | Table: symptom / cause / fix |
| 8 | Escalation | Panel with owners and channels |

Warnings go before the step that needs them, never after. A rollback section that
is hard to find is a rollback section that does not exist.

---

## 4. Project / initiative overview

For status pages and stakeholder-facing summaries.
Typical sources: a slide deck, status docs, plans.

| # | Section | Treatment |
|---|---|---|
| 1 | At a glance | Two- or three-column metric cards |
| 2 | Status strip | RAG lozenge, phase, target date |
| 3 | Objective | Info panel |
| 4 | Scope | Two-column: in scope / out of scope |
| 5 | Milestones | Table with status lozenges |
| 6 | Risks and blockers | Table with severity lozenges |
| 7 | Team and owners | Table, with user mentions where known |
| 8 | Detail | Expands per workstream |

This archetype is read by people who will not scroll. Everything that matters lives
above the milestones table.

---

## Slide decks are a special case

A PowerPoint converts to markdown as a flat sequence of slide titles and bullets.
Do not mirror that structure — one heading per slide produces a shapeless page.

Instead: read all slides, identify the three to six themes the deck actually argues,
and rebuild the page around those themes. Merge slides that make the same point.
Promote the deck's conclusion slide to the summary panel at the top. Speaker notes
frequently contain the real reasoning that the slides only gesture at — mine them.
