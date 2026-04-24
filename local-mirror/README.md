# Local Mirror

Flat markdown backup of the Notion journal. Pure durability layer.

**Derived.** Everything in here is overwritten from Notion on every sync. Don't hand-edit — changes will be lost.

## Structure

```
local-mirror/
├── README.md        # This file
├── summary.md       # Mirror of the Notion "Journal Summary & Context" page
└── entries/
    └── YYYY-MM-DD-slug.md   # One file per journal entry
```

## Filename convention

`YYYY-MM-DD-slug.md` where the slug is the entry title lowercased, punctuation and spaces replaced with hyphens, non-ASCII stripped.

Example: `"The Week That Shifted"` on `2026-01-15` → `2026-01-15-the-week-that-shifted.md`

## When this refreshes

- Automatically at the end of every journaling session
- On demand when you ask Claude to "refresh the mirror" or "sync my journal"

See `../SYSTEM_PROMPT.md` → **Mirrors → Refresh procedure** for the exact spec.
