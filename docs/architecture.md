# Architecture

`cv-forge` is two skills connected as a pipeline, plus the assets they use. It is
distributed as agent skills, not as a standalone application.

## The pipeline

```
                 guided interview                 profile + job posting
   You  ───────────────────────────▶  cv-profiler ───────────▶  Career Profile
                                                                     │
   Job posting  ──────────────────────────────────────────────────▶ │
                                                                     ▼
                                                                 cv-tailor ──▶  CV (+ cover letter)
```

- **`cv-profiler`** interviews the user and produces/maintains the **Career Profile** — one structured Markdown document that is the single source of truth. It saves incrementally and can resume an interrupted session.
- **`cv-tailor`** reads the Career Profile — and, optionally, a job posting — and generates a CV drawing only from the profile: tailored to the role when a posting is given, otherwise a complete general CV.

The Career Profile is the **contract** between the two skills: `cv-tailor` locates content by the section names and conventions that `cv-profiler` writes. That contract is specified in [`career-profile.md`](career-profile.md).

## Source of truth, not a database

There is no structured data layer (no JSON/YAML) and no automated validation. The
Career Profile is Markdown/prose. Consistency is maintained by **conventions the
skills follow while writing** (dates, language levels, metric units, primary-source
propagation), not by a validator. This keeps the profile human-readable and editable
by hand.

## Distribution

`cv-forge` does **not** ship its own installer. The repository is a collection of
skills — each a folder under `skills/` containing a `SKILL.md` — installed with the
shared `skills` CLI:

```bash
npx skills@latest add gianmadd/cv-forge
```

The shared installer places skills in `~/.agents/skills/` and symlinks them into
Claude Code and other AI coding agents — so **multi-tool support comes for free**,
with nothing for us to maintain. Claude Code is the first-class
target where the skills are developed and tested; other agents are validated
incrementally.

Because the skills are interactive (a multi-turn interview, file generation), they
fit agents that support invokable skills/commands well. Tools that only accept
passive context ("rules") are best-effort.

## Repository layout

```
README.md
CHANGELOG.md
CONTEXT.md              # shared vocabulary
package.json            # metadata / versioning (no bin — no custom installer)
docs/                   # this documentation
skills/
  cv-profiler/
    SKILL.md            # skill 1 (the interview)
    references/         # disclosed reference: probe banks, Re-Run migration, worked example profiles
  cv-tailor/
    SKILL.md            # skill 2 (the generator)
    references/         # output-format reference disclosed from SKILL.md
    templates/          # CV / cover-letter templates — inside the skill so they install with it
```

Anything a skill needs **at runtime** lives inside its own folder, because the installer
copies only `skills/<name>/`. So the templates sit under `skills/cv-tailor/templates/` and
the worked example profiles (used as disclosed few-shot) under
`skills/cv-profiler/references/` — each ships with its skill. There is no repo-root
`templates/` or `examples/`: anything the skills read while running must travel with them.

Skills are authored as Markdown (`SKILL.md`), in English for portability; the
*interview and output* are multilingual (see [`principles.md`](principles.md)).
