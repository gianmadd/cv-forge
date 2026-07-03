# Design Decisions

The decisions that shape `cv-forge`, each stated as a fact with its rationale. This
is the durable record — it exists so the design can be understood without any process
notes.

Baselines that frame everything below: the Career Profile is Markdown/prose with **no
structured data layer and no automated validation**; consistency comes from
conventions the skills follow while writing. Skill prompts are written in English;
interaction and output are multilingual.

---

## 1. Scope & distribution

- **The project targets CVs specifically, not generic multi-output.** Without a
  structured data layer, "staying generic" would only produce vaguer questions with no
  real reuse; optimizing for one clear goal is better.
- **The two skills are one pipeline with a shared contract.** `cv-profiler`'s output
  section names and conventions are exactly what `cv-tailor` reads. They are designed
  and changed together, never in a way that leaves the pipeline half-updated.
- **Distribution reuses the shared `skills` installer — we do not build our own.**
  The repo is a collection of `SKILL.md` skills installed via
  `npx skills add gianmadd/cv-forge`. The installer handles
  multi-tool targeting (Claude Code and other agents) for free. *Why:* far
  less to build and maintain, and multi-tool support is immediate. Claude Code is the
  first-class, tested target.

## 2. Career Profile structure

- **Fixed core (nine sections) + conditional sections.** A predictable minimum for
  everyone, plus specialist sections included only when pertinent. *Why:* a purely
  adaptive interview needs an adaptive output, or a plumber ends up with an empty
  "Compliance" section — but a guaranteed core keeps results predictable.
- **Core sections are removable only on explicit user request; conditionals are
  proposed with confirmation.** *Why:* protects the user's control in both directions —
  the skill never quietly drops a core section or forces an extra one.
- **`Address History` removed; `Career Objectives (Historical)` kept as a low-priority
  conditional; `References` added as a conditional.** *Why:* Address History only held
  the current location, already in Contact Information (redundant). Historical
  objectives are a zero-cost opt-in occasionally useful to career changers. References
  are expected in some markets (UK, academia).
- **"Legacy skills" generalized and renamed `Archived / Excluded from CV`.** *Why:* for
  many trades skills don't "go obsolete"; the useful, universal idea is "kept for
  reference but never shown on a CV." It stays conditional and keeps its binding
  exclusion note.
- **Intentional duplication is kept, with Work Experience as the primary source.**
  *Why:* aggregate views (summaries, metrics) help `cv-tailor`; removing them would
  hurt output. Without a validator, a "who owns which fact" convention plus edit-time
  propagation is the best available safeguard.
- **Two note mechanisms, kept separate:** inline binding `> Agent Note:` (per-entry,
  read by `cv-tailor` in place) and the strategic `Notes for CV Customization` section.
  *Why:* centralizing inline notes into the strategic section would detach them from
  the entry they govern and break their binding semantics.
- **`PURPOSE` comments are kept and extended** to mark `[CORE]`/`[CONDITIONAL]` and, for
  conditionals, the pertinence trigger. Exact wording is written during implementation
  against a fixed rule, not pre-approved. *Why:* several `PURPOSE` texts depend on other
  decisions; fixing wording early would only reopen it.

## 3. Interview design

- **Light branching via a single open calibration question (`Phase 0`).** The user
  freely describes their path; the answer tunes tone, examples, and section proposals —
  no rigid sub-interviews, no closed category list. *Why:* a closed taxonomy is exactly
  what creates unmanageable "in-between" profiles; free-form handles hybrids and keeps
  branching to pure calibration. The result is stored as a short **persistent profile
  note** at the top of the document, and the skill is instructed to adapt probes to the
  emerged domain rather than reuse the prompt's built-in tech examples verbatim.
- **Career changers and early-career get shifted emphasis, not a separate branch** —
  transferable skills + transition rationale; studies, projects, internships,
  volunteering, transferable part-time work. A concrete probe bank covers these. *Why:*
  those with the least formal experience need *more* probing to surface value, not a
  dismissive "keep it short."
- **Career gaps are noticed by soft observation, not validation.** In Work Experience
  the skill compares consecutive role dates; for a gap beyond ~6 months it asks once,
  with tact, and never insists; if documented, the entry is neutral, never an excuse.
  Once a gap is addressed (documented or declined) it's remembered so a resumed session
  doesn't re-ask. *Why:* observing dates while writing is not the automated post-hoc
  validation the baseline rules out.
- **"Probe once, then move on."** The skill probes each topic once; if there's nothing
  there, it moves on. *Why:* prevents the added early-career probing from dragging out
  the interview for people who have little to add.
- **Incremental saving (write early, write often)** with `[TO COMPLETE]` on unfinished
  core sections only, and a three-way dispatch (New / Resume Draft / Re-Run). *Why:* a
  30–60 minute interview will be interrupted; losing everything was the original's worst
  flaw. Placeholders only on core avoids marking sections that may never exist.
- **Old-format profiles migrate non-destructively** via an alias map in Re-Run. *Why:*
  renamed sections must not read as "missing" and cause duplication.

## 4. De-biasing & internationalization

- **US-specific fields are contextual, not default.** GPA and security clearance are
  raised only when relevant; work authorization / visa / relocation are optional and
  context-triggered; time zones are out of scope.
- **De-biasing also adds.** Where a target market expects them, the skills raise
  optional personal fields (photo, date of birth, nationality, marital status) and a
  data-processing consent line for markets where it's customary. Always optional,
  market-triggered. A photo can't live in Markdown, so the profile stores a path/note
  and the image is handled at generation. *Why:* removing US fields alone would just
  swap one bias for another; genuine internationalization is symmetric.
- **Action verbs in the generator adapt to the domain** rather than a fixed
  tech-flavored list.
- **The CV template stays a single neutral layout with minor tweaks** (neutral labels,
  Projects made explicitly optional) rather than per-profile templates. *Why:* the
  template structure is already largely neutral; the real output bias lived in verbs and
  the example, and multiple templates would be premature over-engineering.

## 5. Multilingual output

- **The Career Profile is a single document; multilingual is an output concern.** The
  CV's language is chosen at generation and `cv-tailor` translates content into it
  (rendering, not fabrication). Parallel language versions of the profile are out of
  scope. *Why:* two source documents would create a worse drift/sync problem than the
  intentional duplication already carries.
- **`cv-tailor` works in the output language,** maps job-posting keywords across
  languages, flags any it can't map (no silent failure), uses localized section names
  (asking the user which convention), and applies the target language's CV-writing
  conventions rather than English-specific rules verbatim.
- **Web-search assist searches in the relevant language/market** and never for metrics.

## 6. Privacy

- **Consent before storing sensitive data.** A one-time notice at the start that the
  profile is stored locally in clear text, plus per-item consent before recording
  special-category data, with a neutral-version or omission option.
- **Local-only, not git-dependent.** The profile lives only on the user's machine;
  nothing leaves it except explicit web searches (never containing personal data). These
  protections don't rely on a `.gitignore` — the user might not use git at all. The
  end-user's own `.gitignore` is out of scope for this project.

## 7. Content integrity

- **Zero fabrication** is absolute: no invented, estimated, or embellished facts.
- **Rephrasing is allowed by default** for form (tone, syntax, grammar, completeness,
  clarity) — a useful builder improves how things read. The invariant is that
  rephrasing never changes the facts or their meaning. *Why:* the original rule
  ("never edit the user's words without permission") was too strong; what must be
  protected is the content, not the exact words.

## 8. Verification

- **A lightweight eval on non-technical profiles:** 2–3 scripted personas (e.g. nurse,
  plumber, teacher) plus an acceptance checklist (no tech jargon leaking into probes, no
  forced empty core section, conditionals proposed by the rubric, Languages handled).
- **An end-to-end test** (`cv-profiler` → `cv-tailor` → PDF) on at least one technical
  and one non-technical profile: no section lost (Languages included), the PDF compiles,
  and the non-technical CV isn't tech-flavored. *Why:* the whole value is "does it
  really de-bias?" — that needs to be checked, not assumed.

## 9. Examples & few-shot

- **Two example Career Profiles ship in the new structure — one technical, one
  non-technical** (reusing the eval personas). *Why:* a single technical example would
  re-bias the skills toward the exact profile we're de-biasing away from; two examples of
  different range teach the intended breadth.

## 10. Repository, packaging & authorship

- **The repo uses a conventional skills-repository layout:** `skills/<name>/SKILL.md`,
  `docs/`, `CONTEXT.md`, `CHANGELOG.md`, `package.json` (metadata/versioning, no `bin`),
  `examples/`. Skills use the **Skills feature (`SKILL.md`)**, not subagents in
  `.claude/agents/`.
- **Runtime assets live inside the skill folder.** The installer copies only
  `skills/<name>/`, so anything a skill reads while running ships with it: CV /
  cover-letter templates live in `skills/cv-tailor/templates/`, not at the repo root.
  `examples/` stays at the root because it is dev-time reference, not read at runtime.
  *Why:* a repo-root `templates/` would simply not travel with the installed skill.
- **`SKILL.md` frontmatter is `name` + `description` only.** These are the two fields
  the shared `skills` installer requires; we deliberately add nothing else (no
  `allowed-tools`, `license`, `version`) so the skills stay portable across every agent
  the installer targets. No plugin manifest is needed — the installer discovers skills
  by the `skills/<name>/SKILL.md` layout. *Why:* extra frontmatter fields are
  agent-specific and would only risk breaking portability for no gain.
- **Both skills are model-invoked via their `description`, and user-invokable** (in
  Claude Code, `/cv-profiler` and `/cv-tailor`). Neither is hidden/`internal`. The
  `description` is written trigger-rich so the model activates the skill on the right
  intent and not otherwise. *Why:* the skills are user-facing workflows; there is no
  reason to hide either, and a good description gives both discovery paths for free.
- **Each `SKILL.md` is self-contained.** The installer copies only the skill folder, so
  `docs/` is not present at runtime; every rule a skill needs while running is written
  into its `SKILL.md` (docs remain the dev-time record and must stay consistent with it).
  *Why:* a skill that referenced `docs/` would silently lose its rules once installed.
- **The CV / cover-letter templates may be based on an existing template.** They need
  not be written from scratch; a proven layout can be adapted.
- **The template is a single-column LaTeX CV compiled with `pdflatex`.** A single neutral
  layout (`article`-based, standard headings, real selectable text) serves every profile.
  *Why:* `pdflatex` is universal and the Overleaf default (no XeLaTeX toolchain), a
  single column parses cleanly in screening tools, and plain macros are trivial for
  `cv-tailor` to fill. The glyph-to-Unicode map (`\input{glyphtounicode}`,
  `\pdfgentounicode=1`) keeps the PDF's text extractable.
- **`cv-tailor` emits the filled `.tex` (plus any assets); compiling to PDF is a separate
  step.** The user compiles on **Overleaf** (cloud, zero local install) or with a local
  `pdflatex`. *Why:* the skill runs on the user's machine and should not force a heavy
  TeX install; the source it emits compiles anywhere, Overleaf included.
- **Licensed under MIT** (© 2026 Gian Marco Addati).
- **ATS-readability is a goal, owned at the template level.** The product's promise is
  CVs that automated screening tools parse cleanly. `cv-tailor` renders faithfully into
  the template; ensuring the template is itself AI-/ATS-readable is a property of the
  template, verified when it's added — not a constraint `cv-tailor` re-derives.
