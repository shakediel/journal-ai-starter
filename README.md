# journal-ai (starter)

A Claude-powered personal journaling system. Clone this, wire it up to your Notion, and run sessions with Claude as your journaling partner. Entries live in Notion; a flat markdown backup and an Obsidian vault are regenerated from Notion at the end of every session.

This is the empty starter. Nobody else's data is in here.

## What you get

- **`SYSTEM_PROMPT.md`** — the core instructions Claude loads at the start of every session. Defines its role (capture + surface), its modes (Socratic / MI / Stoic with CBT, IFS, coach as needed), the entry structure, the closing ritual, and the refresh procedure for both mirrors.
- **`skills/`** — four Cowork skills:
  - `journal-review/` — weekly/monthly/quarterly reviews across entries
  - `journal-search/` — retrieve past entries by topic, person, tag, or keyword
  - `sync-journal/` — regenerate both mirrors from Notion on demand
  - `obsidian-entry-style/` — format arbitrary prose into a vault-style entry
- **`local-mirror/`** — flat markdown backup of Notion. Pure durability. Regenerated on sync.
- **`obsidian-vault/`** — richer view with YAML frontmatter, `[[wikilinks]]`, and dedicated `People/` + `Threads/` notes. Entries and summary are regenerated on sync; People and Threads are user-owned and never overwritten.

## Setup

### 1. Set up Notion

Create these two things in your Notion workspace:

**a) A parent page** called whatever you want — e.g. "Personal Journal". Copy its URL.

**b) A database** under that parent called "Journal Entries" with these properties:

| Property | Type         | Options                                                          |
| -------- | ------------ | ---------------------------------------------------------------- |
| Entry    | Title        | —                                                                |
| Date     | Date         | —                                                                |
| Mood     | Select       | `great`, `good`, `okay`, `rough`, `bad`                          |
| Tags     | Multi-select | `reflection`, `gratitude`, `goals`, `vent`, `ideas`, `personal`  |

**c) A summary page** under the parent called "Journal Summary & Context". This is the living memory file — Claude reads it at the start of every session and updates it at the end. You can seed it from `starter-templates/Summary & Context seed.md` (see below) or leave it blank the first time.

Copy three IDs:
- **Parent page URL** (the "Personal Journal" page)
- **Summary page ID** (the Summary & Context page — from its URL)
- **Journal Entries data source ID** (from Notion API or by asking Claude to find it with the Notion connector)

### 2. Fill in your placeholders

Find-and-replace these tokens across the repo (they appear in `SYSTEM_PROMPT.md`, the four `skills/*/SKILL.md` files, and the starter templates):

| Token                              | Replace with                                         |
| ---------------------------------- | ---------------------------------------------------- |
| `{{USER_NAME}}`                    | Your name or nickname                                |
| `{{USER_PRONOUNS}}`                | `he/him`, `she/her`, `they/them`, etc.               |
| `{{PARENT_PAGE_URL}}`              | Your Notion parent page URL                          |
| `{{SUMMARY_PAGE_ID}}`              | Your Summary & Context page ID                       |
| `{{JOURNAL_ENTRIES_DATA_SOURCE}}`  | Your Journal Entries data source ID                  |
| `{{REPO_PATH}}`                    | Absolute path to this repo on your machine           |

### 3. Seed the Summary & Context page

Open `starter-templates/Summary & Context seed.md`, copy it into your Notion summary page, fill in the Who / Key Life Context sections with your actual details, and leave Open Threads / Entry Log empty. Claude will grow them over time.

### 4. Connect Claude

Use the Claude Desktop app (Cowork mode) or Claude Code with the Notion MCP connector enabled. At the start of a session, tell Claude to load `SYSTEM_PROMPT.md`. It'll read your Summary & Context page and kick off.

### 5. (Optional) Install the skills

Each folder in `skills/` has a `SKILL.md` you can install as a Cowork skill for automatic triggering. Package with the `skill-creator` skill or your plugin tooling of choice.

## Source of truth

**Notion is the source of truth.** Both local mirrors are derived — regenerated from Notion at the end of every session. When Notion and a mirror disagree, Notion wins. You can edit Notion directly any time; the next sync picks it up.

People notes and Thread notes in the Obsidian vault are the exception — they're user-owned. Claude creates stubs but never overwrites them.

## Refreshing manually

Any time you say "refresh the mirror" or "sync my journal", Claude pulls from Notion and regenerates both mirrors. The closing ritual of a session does this automatically.

## Privacy

This starter ships empty. If you fork it and make it your own, consider keeping your fork private — your journal is in Notion, but the mirrors will sync to disk and into git history if you commit them.

## Credits

Forked from a personal setup. Make it yours.
