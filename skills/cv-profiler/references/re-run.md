# Re-Run details — migration and keeping duplicated facts in sync

Reach for this on the **Re-Run** branch (a complete profile, no `[TO COMPLETE]`). Read
the file fully, preserve everything, and ask what to add or change — never rebuild from
scratch.

## Old-format migration

If a Re-Run profile uses section names from an older structure, recognise them via this
alias map and offer a **non-destructive** migration to the current names. Never treat a
renamed section as missing — that causes duplication.

| Old name | Current name |
| --- | --- |
| `Address History` | *(removed — its only real content, current location, lives in Contact Information)* |
| `Legacy Skills` | `Archived / Excluded from CV` |
| `Summary` / `Professional Summary` | `Professional Summaries` |
| `Objective` / `Career Objective` | `Professional Identity & Positioning` (target/value) or `Career Objectives — Historical` (dated past objectives) |

For an unfamiliar old section, ask the user how it maps before moving any content.

## Keeping duplicated facts in sync

**Work Experience is the primary source.** Professional Summaries, Key Achievements &
Metrics, and Skills / Competencies are aggregate views that intentionally restate facts
living in full in Work Experience.

On a Re-Run, when a fact changes in Work Experience, **propagate** the change to every
aggregate section that repeats it. (On a first build there is nothing to propagate — the
aggregate sections are derived once at the end.)
