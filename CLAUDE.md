# CLAUDE.md

Guidance for working **on** the `cv-forge` repo with Claude Code. This is dev-time
guidance for maintaining the project; it is not shipped to users.

## What this repo is

Two skills forming a pipeline: `cv-profiler` builds a **Career Profile** (the user's
single source of truth) through an interview; `cv-tailor` turns that profile plus a job
posting into a tailored CV. Read `docs/architecture.md` for the big picture and
`docs/decisions.md` for why things are the way they are.

## Layout

- `skills/<name>/SKILL.md` — the skills (`cv-profiler`, `cv-tailor`); one folder per skill.
- `docs/` — project documentation.
- `templates/` — CV / cover-letter templates used by `cv-tailor`.
- `examples/` — example Career Profiles.
- `CONTEXT.md` — shared vocabulary.

## Before changing a skill

Read `docs/principles.md` and `docs/career-profile.md`. Changes must stay consistent
with them.

## Invariants — do not break

- **Prompts in English.** The interview and the output are multilingual, but the
  `SKILL.md` instructions themselves stay in English.
- **Zero fabrication.** Skills never invent, estimate, or embellish facts. Wording may
  be polished; facts and their meaning may not change.
- **The Career Profile is the contract** between the two skills. If you rename a
  section, marker, or convention in one skill, update the other skill **and**
  `docs/career-profile.md` in the same change — a mismatch silently breaks the pipeline.
- **Original work.** No references in the repo to any baseline material, other
  repositories, or third-party works.
- **Never commit personal data** — real Career Profiles, generated CVs, or `output/`.

## Keep in sync

- Add / rename / remove a skill → update `README.md` (link the skill name to its `SKILL.md`).
- Change a skill's behaviour → update the relevant `docs/` page and add a `CHANGELOG.md`
  entry under `[Unreleased]`.
- Complete or reshape planned work → update `docs/roadmap.md`.

## To settle when authoring the skills

- The exact `SKILL.md` frontmatter the shared `skills` installer expects, and whether a
  plugin manifest is required.
- Each skill's invocation model: user-invoked (reachable only by the human) vs
  model-invoked.
