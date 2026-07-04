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

### Decided
- `SKILL.md` frontmatter is `name` + `description` only; both skills are model-invoked and user-invokable; each `SKILL.md` is self-contained (see `docs/decisions.md` §10).
- Output stack: single-column `pdflatex` template; `cv-tailor` emits the filled `.tex` and the user compiles on Overleaf or locally (see `docs/decisions.md` §10). Target deliverable formats: PDF + DOCX, with `.tex` as source (Markdown excluded); DOCX rendering is a roadmap item.

### Notes
- Full verification (scripted eval + non-technical end-to-end) and publishing are still to come — see `docs/roadmap.md`.
- Local `pdflatex` compilation is not yet tested (only Overleaf) — tracked in `docs/roadmap.md`.
