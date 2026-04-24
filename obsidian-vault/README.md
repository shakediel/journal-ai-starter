# Obsidian Vault

A richer view of the journal using wikilinks, graph view, and dedicated notes for people and open threads.

## Status

**Notion is the source of truth.** This vault is a derived view plus some user-owned notes.

## How to use

1. Open Obsidian → Open folder as vault → pick this folder (`obsidian-vault/`).
2. Click any `[[wikilink]]` to jump between notes.
3. Open the graph view (Ctrl/Cmd+G) to see connections across entries, people, and threads.
4. Search with regex (Ctrl/Cmd+Shift+F).

## What's in here

- `Summary & Context.md` — Notion summary page converted to Obsidian style, with `[[wikilinks]]` to people and threads. **Derived** — overwritten on sync.
- `Entries/` — journal entries with YAML frontmatter and wikilinks. **Derived** — overwritten on sync.
- `People/` — notes for people who appear in entries. **User-owned** — Claude creates a stub if a new name appears, but never overwrites a note that already exists. Add memories, context, whatever you want.
- `Threads/` — notes for open threads from the summary. **User-owned** — same rule as People.

## Divergence policy

| Folder / file              | Behavior on sync                                     |
| -------------------------- | ---------------------------------------------------- |
| `Entries/`                 | Overwritten from Notion                              |
| `Summary & Context.md`     | Overwritten from Notion                              |
| `People/`                  | Stubs created for new names; existing notes untouched |
| `Threads/`                 | Stubs created for new threads; existing notes untouched |
| Anything else you add      | Never touched                                        |

## Frontmatter schema

Entries:
```yaml
---
date: YYYY-MM-DD
mood: great | good | okay | rough | bad
tags: [reflection, gratitude, goals, vent, ideas, personal]
source: notion
---
```

People:
```yaml
---
type: person
---
```

Threads:
```yaml
---
type: thread
status: open | open_unexplored | closed
---
```

## File naming

Simple names for cross-OS safety:

- **Entries**: `YYYY-MM-DD Title.md` (strip `:`, `/`, `?`, `*`, `<`, `>`, `|`, `"`, collapse whitespace)
- **People**: first name only (`Alex.md`, `Sam.md`) — matches wikilinks in entries
- **Threads**: exact thread title (`The Fence.md`)

## Git

This vault is committed to the repo. The per-machine `.obsidian/` config folder is gitignored (workspaces, cached plugin state, etc.) so different machines don't fight over it.
