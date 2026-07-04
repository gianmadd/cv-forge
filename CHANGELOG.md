# Changelog

All notable changes to `cv-forge` are documented here.

The format follows common changelog conventions, and the project aims to follow
semantic versioning.

## [Unreleased]

### Added
- Project scaffold: repository structure, `package.json`, `CONTEXT.md`.
- Documentation set under `docs/` (architecture, Career Profile spec, principles, decisions, roadmap).
- Design decisions captured with rationale in `docs/decisions.md`.
- `cv-profiler` skill (`skills/cv-profiler/SKILL.md`): the interview — dispatch (New / Resume / Re-Run), Phase 0 calibration, the nine-core + conditional structure with `PURPOSE` markers, gap noticing, incremental saving, internationalisation, and zero-fabrication rules. Branch-specific reference (probe banks, Re-Run migration) disclosed under `references/`.
- `cv-tailor` skill (`skills/cv-tailor/SKILL.md`): the generator — reads the profile by its contract, cross-language keyword matching with no silent failure, domain-adaptive selection, localised rendering, market-appropriate fields, and ATS-readable compilation (output-format reference under `references/`).
- CV and cover-letter templates (`skills/cv-tailor/templates/resume.tex`, `cover-letter.tex`): single-column, `pdflatex`, ATS-oriented (`glyphtounicode`), with a placeholder fill contract. Preview verified on Overleaf.
- LaTeX special-character escaping rule for `cv-tailor` (SKILL.md + `references/output-format.md`): profile text with `& % $ # _ ~ ^ \ { }` is escaped when filling the template, so generated CVs compile. Surfaced by the first end-to-end test.
- Two example Career Profiles under `skills/cv-profiler/references/`, wired into `cv-profiler` as disclosed few-shot: a senior AI/ML engineer (technical) and an experienced physician (non-technical).
- `cv-tailor` general mode: with no job posting it produces a complete, high-quality general CV from the profile's own positioning; a job posting only sharpens it for a specific application (tailored mode).
- Match-transparency report in `cv-tailor` (Step 3): each posting requirement is classified three ways — *covered* / *present but not yet surfaced* / *genuine gap* — with a plain count and **no match percentage or fit score**. Reframes "no silent failure" as an actionable split (surface existing content vs. a gap only the user can close). See `docs/decisions.md` §7.
- Delivery-time self-check gate in `cv-tailor` (Step 7): an explicit zero-fabrication checklist (profile-only sourcing, no proxy metrics, nothing from Archived/Excluded, Agent Notes honoured, gaps reported, clean render) confirmed before handing over the CV.
- Sharpened zero-fabrication wording in `cv-tailor`: names the evasion tactics it must avoid (no proxy metrics, never "estimate conservatively", unbacked keywords omitted not invented).
- LaTeX template polish: `\urlstyle{same}` in `resume.tex` and `cover-letter.tex` (URLs in the body font, cleaner and parse-safe); and a `~`-as-"approximately" → `$\sim$` guidance added to the escaping rules in `references/output-format.md`.
- Currency/non-ASCII symbol handling: templates load `textcomp`, and `references/output-format.md` maps common symbols to safe macros (`€` → `\texteuro`, `£` → `\textsterling`, `±` → `$\pm$`, …). Surfaced by the non-technical (Italian, humanities) end-to-end test, where a bare `\euro` would have failed to compile.

### Decided
- `SKILL.md` frontmatter is `name` + `description` only; both skills are model-invoked and user-invokable; each `SKILL.md` is self-contained (see `docs/decisions.md` §10).
- Output stack: single-column `pdflatex` template; `cv-tailor` emits the filled `.tex` and the user compiles on Overleaf or locally (see `docs/decisions.md` §10). Target deliverable formats: PDF + DOCX, with `.tex` as source (Markdown excluded); DOCX rendering is a roadmap item.
- Compile hand-off decided (see `docs/decisions.md` §10): `cv-tailor` offers both routes and explains each — **Overleaf** (zero-install default, realistic for non-technical users) or **local compilation** — and, when a local `pdflatex` is present, offers to compile and hand over the PDF rather than compiling silently. Local dependencies are **scoped to the chosen template** (TinyTeX as the light route; `texlive-latex-base` alone is insufficient because of the template's fonts and icons, and Tectonic is off-target because its XeTeX engine can't compile the template's pdfTeX `glyphtounicode` primitives).

### Notes
- A non-technical end-to-end has now passed (Italian literature-professor profile → `cv-tailor` → PDF), surfacing the currency-symbol fix and confirming de-biasing on a humanities profile; the scripted persona eval (exercising `cv-profiler`'s interview) and publishing remain — see `docs/roadmap.md`.
- Local `pdflatex` compilation now exercised via TinyTeX (~300 MB) on the non-technical profile — clean 1-page PDF; wiring the detect-and-offer behaviour into `cv-tailor` and the dependency-structuring approach remain — see `docs/roadmap.md`.
