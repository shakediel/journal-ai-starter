---
name: obsidian-entry-style
description: Format a piece of content (a new journal entry, a pasted block of text, a review output, or any other prose) into the user's Obsidian vault entry style — YAML frontmatter with date/mood/tags/source, H2 section headers in their voice, `[[wikilinks]]` applied to people and open threads, simple cross-OS-safe filename `YYYY-MM-DD Title.md`, and the closing `---` + `*To be continued.*` marker. Use whenever the user says "format this as an Obsidian entry", "convert to obsidian style", "make this a vault entry", "add wikilinks to this", "turn this into an Obsidian note", "save this to the vault", or any variant that implies taking raw content and shaping it for the Obsidian vault. Also use when content was produced outside the standard journaling flow (e.g. a chat transcript, a brainstorm, a review from elsewhere) and needs to land in the vault in the right shape. Does NOT write to Notion — this is vault-only formatting.
---

# Obsidian Entry Style

> **Template note:** Replace `{{REPO_PATH}}` before using this skill.

This skill takes arbitrary content and formats it as an entry in the user's Obsidian vault at `{{REPO_PATH}}/obsidian-vault/`. It is the standalone version of the Obsidian formatting rules that are also applied during the end-of-session sync.

## Before anything else

Read the **Obsidian Entry Style** section of `{{REPO_PATH}}/SYSTEM_PROMPT.md`. That section is the source of truth for frontmatter schema, filename rules, wikilink rules, and stub creation. Follow it exactly.

This skill does not duplicate the spec — it applies it to a one-off piece of content.

## Inputs to gather

Before formatting, make sure you have:

1. **The content** — the actual prose to format. Ask for it if it wasn't pasted or generated just now.
2. **A date** — default to today. Ask only if the content is clearly about a different day.
3. **A title** — either user-provided or pulled from the content. Short, evocative, specific (per the Notion Entry Structure rules in SYSTEM_PROMPT.md). Strip forbidden characters for the filename.
4. **Mood** — ask if not clear from context.
5. **Tags** — 1-3 from `reflection / gratitude / goals / vent / ideas / personal`. Infer from content, confirm if uncertain.

If any of these are missing and can't be reasonably inferred, ask for them in a single message — don't interrogate one at a time.

## What this skill produces

1. An entry file at `{{REPO_PATH}}/obsidian-vault/Entries/YYYY-MM-DD Title.md` with:
   - YAML frontmatter (`date`, `mood`, `tags`, `source`)
   - H2 section headers in the user's voice (thematic titles, not generic ones)
   - Body prose with `[[wikilinks]]` applied on first mention of each person (first name) and any open thread referenced
   - Closing `---` then `*To be continued.*`
2. Stubs in `obsidian-vault/People/` and `obsidian-vault/Threads/` for any newly-linked target that doesn't have a note yet (see SYSTEM_PROMPT.md for the stub template). Never overwrite existing stubs.

## What this skill does NOT do

- Does not write to Notion. If the user wants this in Notion too, hand off to a regular journaling session or ask explicitly.
- Does not update the Summary & Context page.
- Does not refresh the flat `local-mirror/`.
- Does not overwrite an existing entry file without asking first — if the filename already exists in `Entries/`, confirm before overwriting.

### Sibling skills — when this is the wrong call

- If the entry you want in the vault **already exists in Notion**, use `sync-journal` instead — it'll regenerate the vault entry with the correct wikilinks automatically. This skill is for content that isn't in Notion (pasted prose, brainstorms, outputs from other conversations).
- If the user wants to **start a fresh journaling session** (Notion + vault + mirror all in one flow), load `SYSTEM_PROMPT.md`.

## Wikilink discipline

- Use canonical names. People get first names (`[[Alex]]`, `[[Sam]]`). Threads use the exact thread title from the summary (`[[The Fence]]`).
- Check `obsidian-vault/People/` and `obsidian-vault/Threads/` for existing filenames before coining a new link target. Match existing filenames exactly.
- Link on first mention only, unless a later section is specifically about that person/thread again.
- If the content references an open thread that doesn't exist yet in `Threads/`, create a stub. If it references a person who doesn't exist in `People/`, create a stub.

## Reporting

> Saved to `obsidian-vault/Entries/<filename>`. N new stubs: [list].

If no stubs were created, omit that line.
