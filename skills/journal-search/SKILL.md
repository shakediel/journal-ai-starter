---
name: journal-search
description: Search the user's personal Notion journal for existing entries matching a topic, theme, person, question, or keyword. Returns actual quoted passages (not summaries or paraphrases) with dates and entry titles so the user can reference what they previously wrote in their own words. Use whenever the user asks "find what I wrote about X", "pull up entries about Y", "search my journal for Z", "did I journal about X", "what did I say last time about Y", "what have I written about [person/topic]", "show me entries tagged X", "find that entry where I talked about Z", or any variant that implies retrieving existing journal content rather than writing new content. Also trigger when they reference a past moment or past reflection mid-conversation and it would be useful to surface the actual passage.
---

# Journal Search

> **Template note:** Replace `{{USER_NAME}}`, `{{REPO_PATH}}`, `{{SUMMARY_PAGE_URL}}`, and `{{JOURNAL_ENTRIES_DATA_SOURCE}}` before using this skill.

{{USER_NAME}}'s personal journal lives in Notion. Entries are in the "Journal Entries" database (data source ID: `{{JOURNAL_ENTRIES_DATA_SOURCE}}`), with a living summary at {{SUMMARY_PAGE_URL}}.

This skill retrieves existing content. It does **not** write new entries and does **not** open a journaling session.

### Sibling skills — when this is the wrong call

- If the user wants a **new meta-entry summarizing a window of time** (weekly/monthly/quarterly review), that's `journal-review` — not this. This skill returns quotes from existing entries; it doesn't produce new ones.
- If they want to **mirror Notion to disk** (refresh local files from Notion), that's `sync-journal`.
- If they want to **start a new journaling session**, load `{{REPO_PATH}}/SYSTEM_PROMPT.md` instead.

Use this skill when the job is *"surface what I already wrote"* and nothing more.

## Query handling

Queries come in several shapes. Handle them differently:

**Keyword / topic**: *"find entries about work"*, *"what have I written about [person]"*
→ Semantic search across all entries. Return everything relevant.

**Tag filter**: *"show me all vent entries"*, *"find entries tagged goals"*
→ Filter the database by the Tags property. Return chronologically.

**Time-bounded**: *"what did I write about the move"*, *"find the birthday entry"*
→ Search by content; if a date is implied, use it as a hint but don't require exact match.

**Question form**: *"what did I decide about X?"*, *"what did I say about Y?"*
→ The question is the query. Extract the core topic and search. Also surface the *implicit* context (e.g., "you didn't decide anything final, but here's what you wrote").

**Fuzzy memory**: *"that entry where I talked about the thing with my boss"*
→ Search on the distinctive phrase. These usually land on a single entry.

## How to search

1. Use the Notion search tool scoped to the Journal Entries data source (`data_source_url: collection://{{JOURNAL_ENTRIES_DATA_SOURCE}}`). For tag/property filters, fetch the data source and filter client-side if needed.
2. For each candidate entry, fetch the full content.
3. Identify the specific passage(s) relevant to the query — typically 1–3 paragraphs.

## Output format

Return the results as a conversational response in this shape:

```
Found N entries matching "<query>":

### <Entry Title> — <Date>
<Mood tag, Tags>

> <Direct quote of relevant passage, preserving voice and formatting>

[Optional: if the entry touches the topic in multiple places, include 2-3 passages separated by ellipses in brackets, like [...]]

---

### <Next Entry Title> — <Date>
...
```

After listing all matches, if the query was a question or implies one, add a short synthesis paragraph:

```
**Across these entries:** <one-paragraph observation about what their previous writing actually says on this topic — not a paraphrase of each entry, but a synthesis across them. Keep it short, use their framings, don't impose your own.>
```

## Rules

- **Quote verbatim.** Don't paraphrase, don't clean up, don't sanitize. Their words are the point.
- **Don't editorialize.** The synthesis at the end is optional and must stay close to what they actually wrote. Don't add interpretation they didn't make.
- **Be specific about dates.** Every passage gets the entry date so they can place it in time.
- **If nothing matches, say so.** Don't manufacture relevance. Offer to broaden the search.
- **If too many match (>5)**, list the titles/dates first and ask which to pull in full.

## What this skill does NOT do

- Does not write new journal entries. If the search turns into a journaling conversation, point back to the main journaling flow.
- Does not modify existing entries.
- Does not update the summary page or the mirror.
- Does not interpret meaning beyond a short synthesis at the end.
