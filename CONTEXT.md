# Context & Vocabulary

Shared terms used across `cv-forge`'s skills and documentation. Both skills and
all docs use these words with these meanings.

- **Career Profile** — the single, structured, comprehensive document a user builds with `cv-profiler`. It is the **source of truth**: every CV is generated from it. It is *not* a CV — it holds far more than any single CV would show.
- **`cv-profiler`** — the first skill. A guided interview that builds and maintains the Career Profile.
- **`cv-tailor`** — the second skill. Reads a Career Profile plus a job posting and produces a CV (and optionally a cover letter) tailored to that role.
- **Job posting** — the description of a specific role a user is applying for; the input that `cv-tailor` tailors against.

### Career Profile structure
- **Core section** — one of the fixed sections present in every Career Profile (see `docs/career-profile.md`).
- **Conditional section** — a specialist section included only when it's genuinely relevant to the person's profile.
- **`[CORE]` / `[CONDITIONAL]`** — markers in each section's `PURPOSE` comment stating its status.
- **`PURPOSE` comment** — an HTML comment under a section heading describing what the section contains and what it does *not* contain.
- **Agent Note** — an inline, per-entry, **binding** instruction (e.g. "team effort — do not claim sole ownership") that `cv-tailor` must respect where it appears. Distinct from the strategic guidance below.
- **Notes for CV Customization** — a core section holding **strategic positioning guidance** (which angle to emphasize for which kind of role). It points to themes; it does not restate content and does not hold per-entry instructions.

### Interview flow
- **Calibration** — the opening `Phase 0` question ("describe the career path you want to document") that adapts the interview's tone, examples, and later section proposals.
- **Profile note** — a short (1–2 line) persistent summary of the calibration answer, kept at the top of the Career Profile so the interview stays adapted and can resume without re-asking.
- **`[TO COMPLETE]`** — placeholder text marking a core section that hasn't been filled in yet. Used for incremental saving and resuming.
- **New Build / Resume Draft / Re-Run** — the three modes `cv-profiler` dispatches into, decided by whether the file exists and whether it contains `[TO COMPLETE]`.

### Cross-cutting
- **Zero fabrication** — nothing is invented, estimated, or embellished; all facts come from the user.
- **Tailoring** — selecting and structuring content from the Career Profile to fit a specific job posting.
- **Target market** — the country/market a CV is aimed at; determines which personal fields are appropriate (see `docs/principles.md`).
