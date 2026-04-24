---
name: journal-review
description: Perform a periodic review of the user's personal Notion journal — weekly, monthly, quarterly, or any custom date window. Reads every entry in the window, surfaces cross-entry patterns, names what moved and what stayed stuck, flags open threads that haven't been touched, and writes a meta-reflection entry in the user's voice. Use whenever the user asks for a "weekly review", "monthly review", "quarterly review", "year-in-review", "review my journal", "look at my recent entries", "what patterns in my writing", "what themes have been coming up", "how have I been doing lately", or any variant that implies looking back across multiple existing journal entries rather than writing a new one from scratch. Also trigger for specific windows like "past two weeks", "the last 30 days" in a review context.
---

# Journal Review

> **Template note:** Replace `{{USER_NAME}}`, `{{REPO_PATH}}`, `{{SUMMARY_PAGE_URL}}`, and `{{JOURNAL_ENTRIES_DATA_SOURCE}}` before using this skill.

{{USER_NAME}} uses Notion for personal journaling. Entries live in the "Journal Entries" database (data source ID: `{{JOURNAL_ENTRIES_DATA_SOURCE}}`). A living summary page at {{SUMMARY_PAGE_URL}} tracks context and open threads.

This skill runs a **review** — not a new journaling session. The goal is to look across multiple entries and produce a single meta-reflection that captures what's been happening at a higher altitude than any individual entry. A review always produces a **new entry** in Notion; if the job is just to surface old writing without creating new content, that's a different skill.

### Sibling skills — when this is the wrong call

- If the user wants to **find/quote existing entries** without producing a new one, that's `journal-search`. This skill would be overkill.
- If they want to **mirror Notion to disk**, that's `sync-journal`.
- If they want to **start a fresh journaling session**, load `{{REPO_PATH}}/SYSTEM_PROMPT.md` instead.

## Before anything else

Read `{{REPO_PATH}}/SYSTEM_PROMPT.md` first. It contains the voice, entry structure, tag definitions, and anti-sycophancy rules that must apply to the review output as well. The review is just another entry — same style, same voice, same discipline.

## Review window

If the user specifies a window (weekly, monthly, "last 30 days", "since April 1"), use it. If not, ask once — briefly: *"What window — last 7 days, last 30, last quarter, or something custom?"* Don't interrogate further once they answer.

Then query the Journal Entries database for entries whose Date falls in that window. Also fetch the Journal Summary & Context page for the open-threads list.

## What to look for

The point of a review isn't to summarize each entry — it's to see things that are only visible from altitude:

1. **Themes that keep reappearing** — topics showing up across multiple entries. Name the theme and the entries it spans.
2. **Movement vs. stagnation** — where did the thinking actually shift vs. where are they circling the same point? Be specific: quote the earlier framing and the later framing side by side if there's a change, or name the loop if there isn't.
3. **Contradictions** — places where they said X in one entry and something in tension with X in another. Don't resolve them; just surface them.
4. **Language patterns** — repeated metaphors, recurring phrases, things they keep almost-saying but pulling back from.
5. **Mood vs. content** — check if selected moods align with what the entries actually contained. Name any divergence.
6. **Open threads** — cross-check the summary's open threads against the entries. Which got touched? Which went quiet? Which grew? Which should be marked resolved?
7. **What's absent** — what are they not writing about that might deserve attention (based on open threads, prior entries, or conspicuous gaps)? Ask this as a question in the review, not a conclusion.

## Review output

Write the review as a new Notion entry in the Journal Entries database, parented under the journal parent page (same as regular entries). Then also output the review text in the conversation so the user can read it immediately.

### Title format

`Review: <Window> <Date Range>` — e.g., `Review: Week of April 14-20, 2026`, `Review: April 2026`, `Review: Q2 2026`.

### Properties

- **Date**: the end date of the review window (today if the window ends today)
- **Mood**: infer from the review's overall tone
- **Tags**: always include `reflection`. Add others only if the review itself is primarily about them.

### Body structure

Use H2 sections. Suggested shape (adapt based on what actually surfaces):

- **What this review covers** — one paragraph naming the window and the entries being looked at by title. Not a summary of each, just the scope.
- **Themes that kept showing up** — 2-4 sections, each named for the theme. In each: what showed up, where, and whether it moved. Quote short phrases from the entries where useful.
- **What shifted** — a section on thinking that actually changed within the window. Be specific about the before and after.
- **What didn't move** — a section on loops that stayed loops. Name them without being cruel, but without softening either.
- **Open threads check** — which threads got touched, which went quiet, which resolved, which grew new branches.
- **What you're not saying** — one section, phrased as questions rather than conclusions.
- **One thing to try before next review** — single specific, actionable, small. Same shape as the session-end closing ritual.

### Voice and rules

- Their voice, in first person ("I"). Same as regular entries. The review is theirs, not a clinical summary.
- Organize, don't sanitize. Rough edges intact.
- No bullet-spam inside sections — prose paragraphs (bullets are fine for the high-level section headings themselves if needed).
- No reassurance-soup. No "you're doing great." No reflexive validation. If a theme is that they're stuck, the review says they're stuck.
- Don't resolve tensions the entries haven't resolved. The review names them; it doesn't close them.
- Close with `---` and `_To be continued._` like regular entries.

## After writing the review

1. Save the review entry to Notion.
2. Update the Journal Summary & Context page: add the review to the entry log, update any open threads based on what the review surfaced (mark resolved, add new ones, note stagnation).
3. Refresh both the local mirror and the Obsidian vault following the Mirrors → Refresh procedure in `{{REPO_PATH}}/SYSTEM_PROMPT.md`. This includes writing the review to `obsidian-vault/Entries/` with wikilinks, and creating stubs for any new people or threads the review surfaces.
4. Report back: *"Review saved — covered N entries, surfaced X themes. Summary updated, both mirrors refreshed."*

## When to push back

If the user asks for a review but there are only 0-1 entries in the window, don't force a review — tell them the window is too thin and offer a larger one. A review over one entry is a recap, not a review.
