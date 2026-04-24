---
name: sync-journal
description: Refresh the user's local markdown mirror (`local-mirror/`) and Obsidian vault (`obsidian-vault/`) from their Notion journal. Pulls the latest summary page and every entry in the Journal Entries database, overwrites derived files, creates stubs for any newly-linked people or threads, and leaves user-owned notes untouched. Use whenever the user says anything that implies "make the local copy match Notion" — including "sync my journal", "refresh the mirror", "refresh everything", "sync obsidian", "refresh the vault", "pull the latest from Notion", "update the local copy", "regenerate entries", "re-export my journal". Also trigger if they mention editing Notion directly and wanting the local copies to catch up, or if they notice a discrepancy between Notion and disk. Reach for this skill even when they don't name the mirror/vault explicitly — any phrasing that means "pull Notion → disk" counts. Does NOT create new journal entries and does NOT start a journaling session; it's read-from-Notion, write-to-disk only.
---

# Sync Journal

> **Template note:** Replace `{{REPO_PATH}}` before using this skill.

This skill refreshes the two local mirrors of the user's Notion journal. It is a **sync**, not a write — it never creates new journal content in Notion. Its only job is to make the local files match Notion.

## Before anything else

Read `{{REPO_PATH}}/SYSTEM_PROMPT.md` and follow the **Mirrors → Refresh procedure** section exactly. That section is the authoritative spec. This skill exists as a standalone entry point so the sync can be triggered outside a journaling session (e.g. after the user edits Notion directly, or midway through a conversation).

Do not duplicate the procedure here. If the spec in SYSTEM_PROMPT.md and this skill ever diverge, SYSTEM_PROMPT.md wins.

## What this skill covers

- Overwrite `local-mirror/summary.md` and `local-mirror/entries/*.md` from Notion.
- Overwrite `obsidian-vault/Summary & Context.md` and `obsidian-vault/Entries/*.md` from Notion, applying Obsidian Entry Style (frontmatter + wikilinks).
- Create stubs in `obsidian-vault/People/` and `obsidian-vault/Threads/` for any newly-linked names or threads that don't have notes yet.
- Never overwrite existing People/Threads notes or any other user-owned file in the vault.
- Never delete orphan files on either side — flag them to the user instead.

## What this skill does NOT do

- Does not create a new journal entry.
- Does not update the Notion summary page.
- Does not run the closing ritual of a journaling session.
- Does not delete local files that are missing in Notion.

### Sibling skills — when this is the wrong call

- Want to **write a new entry**? Start a journaling session (`SYSTEM_PROMPT.md`) — it syncs at the end anyway.
- Want to **produce a periodic review entry**? Use `journal-review`.
- Want to **find quotes from existing entries**? Use `journal-search`.
- Want to **format a one-off piece of content as an Obsidian entry** (e.g., pasted prose that isn't yet in Notion)? Use `obsidian-entry-style`. This skill only mirrors what's already in Notion — it can't create vault entries from thin air.

## Reporting

At the end, report briefly:

> Refreshed — N entries (mirror + vault), summary updated, M new stubs.

Only break out per-file detail if something failed. If orphan files were found locally, list them and ask the user what to do.

## When to refuse

If Notion is unreachable or returns nothing, don't overwrite local files with empty content. Tell the user the fetch failed and stop.
