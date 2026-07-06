# The Career Profile

The Career Profile is the structured Markdown document `cv-profiler` builds and
`cv-tailor` reads. This page specifies its structure and conventions — the contract
between the two skills.

It is a **superset**: it holds everything about the user's career, far more than any
single CV shows. `cv-tailor` selects from it per job.

## Sections

### Core sections (always present)

Every Career Profile contains these nine, in this order. They are a guaranteed
default; they may be removed or restructured **only at the user's explicit request**,
never on the skill's initiative.

1. **Contact Information**
2. **Professional Identity & Positioning** — target titles + value proposition (not a summary of experience; target titles stay within the roles/level the user named — broadening needs their confirmation)
3. **Professional Summaries** — a few genuinely different summary angles
4. **Skills / Competencies** — capability categories adapted to the user's field
5. **Work Experience** — the canonical source for all role detail
6. **Education & Training**
7. **Key Achievements & Metrics** — a curated highlight reel, not a repeat of every bullet
8. **Languages** — languages + self-assessed level; appears in a CV only when there's more than a native language
9. **Notes for CV Customization** — strategic positioning guidance (points to themes; no per-entry instructions)

### Conditional sections (included only when relevant)

Proposed by the skill **only when genuinely pertinent** to the profile, and always
with the user's confirmation (see the proposal rubric below). Examples:

Hybrid Strengths · Industries Supported · Publications & Writing · Leadership & Soft
Skills · Project Highlights · Compliance & Framework Expertise · Volunteer Work ·
References · Work Preferences (home of relocation / work authorization) · Career
Objectives — Historical (kept, but very low proposal priority) · **Archived /
Excluded from CV**.

> **Archived / Excluded from CV** is the generalized replacement for the old
> "legacy skills" idea: anything the user wants kept for reference but never shown on
> a CV (outdated tools, a former career they don't want surfaced). It carries a
> binding note telling `cv-tailor` to skip it entirely.

Removed from the older design: **Address History** (redundant — current location
already lives in Contact Information).

### Proposal rubric for conditional sections

Propose a conditional section only when **all three** hold:

1. the user has already mentioned concrete content that would fill it (not hypothetical);
2. it's genuinely relevant to the profile/positioning that emerged at calibration;
3. it adds something not already covered by a core section.

Always explain *why* it would help; never propose sections "to fill space." When in
doubt, don't propose.

## Section markers: the `PURPOSE` comment

Each section heading is preceded by an HTML comment describing its role. Every
`PURPOSE`:

1. states what the section contains **and what it does not**;
2. marks the section `[CORE]` or `[CONDITIONAL]`;
3. uses neutral, work-agnostic language (no field-specific examples);
4. for conditional sections, notes in one line the **pertinence trigger** (which kind of profile it's proposed for).

```markdown
<!-- PURPOSE: [CORE] Structured capability categories adapted to the user's field.
     Lists capabilities without re-explaining where they were used. -->
## Skills / Competencies
```

## Two kinds of notes — do not conflate

- **`> Agent Note:` (inline, binding, per-entry).** Sits next to the entry it
  concerns; `cv-tailor` reads it there and must obey it (e.g. "team effort — do not
  claim sole ownership"). Added only with the user's consent, written to be
  human-readable.
- **`Notes for CV Customization` (core section, strategic).** Positioning guidance
  that points to themes and emphasis. It never holds per-entry instructions.

These are different mechanisms. Inline notes stay inline; they are never centralized
into the strategic section.

## Redundancy and the primary source

Some sections intentionally duplicate data (Summaries, Key Achievements, and
Competencies restate things that live in full in Work Experience) — these aggregate
views are useful to `cv-tailor`.

- **Work Experience is the primary source** for role detail.
- **Aggregates add no number that isn't in Work Experience.** A derived section may
  restate and reframe, but must introduce no duration, count, or "X years as Y" that the
  user didn't state or that isn't directly datable from the recorded Work Experience dates.
  Decomposing a stated total into a per-role figure (e.g. "~9 years in the field" → "~6 of
  them as manager") is fabrication, not derivation, and would propagate into every CV.
- When a fact changes in Work Experience on an existing profile, the skill propagates
  the change to the summary sections that repeat it. This propagation applies at
  **edit / Re-Run time**; during a first build the summary sections are simply
  derived at the end, so there is nothing to propagate.

## Incremental saving

`cv-profiler` writes early and often — it does not wait until the end.

- After calibration it creates the file with the **skeleton of the nine core
  sections** and the profile note at the top.
- It updates the file **after each phase** and **after each role** in Work Experience.
- Unfinished **core** sections are marked `[TO COMPLETE]` (empty — to fill).
  Conditional sections are **not** pre-stubbed — they appear only once proposed and
  accepted, so nothing marks a section that may never exist.
- Core sections **seeded from an imported CV** (see Import, in `cv-profiler`) are marked
  `[TO CONFIRM]`: the section already holds extracted content, but it **awaits the user's
  confirmation** — it is shown for review, not asked from scratch. The marker is removed
  once the user confirms the content.

## Draft markers — read by both skills

Two markers describe a section's readiness. Both are **shared contract tokens**: they are
written by `cv-profiler` and also recognised by `cv-tailor`.

| Marker | Meaning | `cv-tailor` behaviour |
| --- | --- | --- |
| `[TO COMPLETE]` | empty — no real content yet | **skip** the section |
| `[TO CONFIRM]` | extracted from an imported CV, present but unconfirmed | **use** the content, with a caveat that it is unconfirmed |
| *(none)* | confirmed content | render normally |

`cv-tailor` **never renders either literal marker** into a CV, and warns the user when it
generates from a draft (any marker still present). This lets an imported profile be turned
into a CV immediately, before the interview confirms every section.

## Provenance comment (documentary only)

When a profile is seeded from an imported CV, `cv-profiler` records the source in a
**separate HTML comment** (distinct from the calibration profile note), e.g.
`<!-- SOURCE: seeded from cv.pdf on 2026-05-01 -->`, and records declined import offers
there once a profile exists. This line is **documentary metadata**: `cv-tailor` ignores it,
and the recorded path is never treated as a live input (it is not a second source of truth).

## Session modes (dispatch)

Decided purely from the file, no validation:

- **No file** → **New Build** (start from calibration; or **Import → New** if the user
  accepts an offer to seed from a CV in the directory).
- **File contains `[TO COMPLETE]` or `[TO CONFIRM]`** → **Resume Draft** (unfinished *or*
  unconfirmed content remains — re-read, recover the profile note, continue from the first
  placeholder; a `[TO CONFIRM]` section is shown for confirmation, not asked from scratch).
- **File with neither marker** → **Re-Run** (enrich/update a complete profile, preserving
  existing content). Within Re-Run the *intent* follows the user's request: plain enrich,
  an on-request **audit**, or **Import → Enrich** when a CV is accepted against the profile.

A complete file later hand-edited to reintroduce `[TO COMPLETE]` is treated as a
draft again — which is the intended behavior.

For a profile written in the **old format**, Re-Run uses an alias map (old section
names → new) to recognize existing content and offer a non-destructive migration to
the new structure.

## Style conventions (followed while writing, not validated)

- Consistent date format throughout (e.g. `Month Year – Month Year`, `Present` for current).
- Uniform language levels (see below).
- Metrics readable and with units (e.g. "reduced latency by 40%").
- Consistent role/company title formatting.
- **Overlapping / concurrent roles are normal**, not an inconsistency: freelance
  alongside employment, portfolio careers, academics splitting research/teaching. The
  interview allows overlapping dates and may group related roles; output orders roles
  by end date and keeps overlaps as separate entries.

### Languages

Every user is asked for their languages and a self-assessed level (CEFR A1–C2, or
native / fluent / intermediate / basic). The level is descriptive and self-assessed —
**no certification is required** (one may be mentioned if held). One scale is preferred,
but **faithful capture of the user's own words wins**: a CEFR grade for one language
alongside "native"/"fluent" for another is kept as stated, not force-converted and not
treated as an inconsistency. A Languages section appears in a generated CV only when there
is something beyond the native language.
