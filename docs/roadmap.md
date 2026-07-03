# Roadmap

What's decided, what's left to build, and what's deliberately deferred.

## Done

- Project direction, scope, and full design — captured in [`decisions.md`](decisions.md).
- Career Profile specification — [`career-profile.md`](career-profile.md).
- Names fixed: repo `cv-forge`; skills `cv-profiler` and `cv-tailor`.
- Distribution model chosen: shared `skills` installer, no custom installer.
- Repo owner (`gianmadd`) and license (MIT, © Gian Marco Addati) decided.
- Repository documentation and scaffold (this `docs/` set, `README.md`, `CONTEXT.md`, `CHANGELOG.md`, `package.json`).
- `SKILL.md` conventions settled: frontmatter (`name` + `description`), invocation model, self-containment — see [`decisions.md`](decisions.md) §10.
- **`cv-profiler` written** (`skills/cv-profiler/SKILL.md` + `references/`): dispatch, Phase 0 calibration, core/conditional structure with `PURPOSE` markers, probe banks, gap noticing, conditional-proposal rubric, incremental saving, zero-fabrication.
- **`cv-tailor` written** (`skills/cv-tailor/SKILL.md` + `references/`): reads the profile by its contract, cross-language keyword matching, domain-adaptive selection, localized rendering, market fields, ATS-readable compilation.
- **CV / cover-letter templates written** (`skills/cv-tailor/templates/`): single-column `pdflatex`, ATS-oriented (`glyphtounicode`); preview verified on Overleaf.

## To build

1. **Examples.** Add two example Career Profiles — one technical, one non-technical.
   (The CV / cover-letter templates are done: single-column `pdflatex`, in
   `skills/cv-tailor/templates/`, satisfying `references/output-format.md`.)
2. **Verification.** Run the lightweight eval (non-technical personas + acceptance
   checklist) and one end-to-end test per profile type.
3. **Flow & operability check.** Walk the full pipeline as an installed skill and confirm
   it runs cleanly at the operational level: file **permissions** for reading/writing the
   profile and the output, whether any **helper scripts** are needed (e.g. a compile
   wrapper, a setup/dependency check for `pdflatex`) or whether the skills stay
   script-free, and that the compile hand-off (Overleaf / local `pdflatex`) is smooth. Fix
   whatever trips the flow.
   - **Local compilation — still to test.** Only the Overleaf path has been exercised so
     far (a local TeX install was flaky this session). Verify the local route end to end:
     `pdflatex resume.tex` run **twice** (for cross-references), then clean the aux files
     (`*.aux *.log *.out *.toc *.fls *.fdb_latexmk`). Decide whether `cv-tailor` should
     **auto-compile locally when `pdflatex` is present** (compile-twice + cleanup, as some
     comparable agents do) rather than only handing off the `.tex`.
4. **Cleanup before publishing.** Tidy internal design notes so the deliverables are
   clean and consistent.
5. **Publish.** Confirm the repo conforms to the `skills` convention, then publish and
   verify installation on Claude Code — followed by incremental checks on other agents.

## Deferred / open questions

- **Let the user choose among several templates.** Ship more than one CV layout and let
  the user pick — ideally pointing them to a gallery/link where they can browse options,
  then generating into the chosen one. For now there is a single template; this is the
  path to multiple.
- **Other agents beyond Claude Code** — handled by the shared installer; only
  *testing* remains, after the skills exist.

## Content still to produce

The skills author `PURPOSE` texts and probes at runtime (the rules and probe banks now
live in the skills). What remains to write as repo assets: the two example Career
Profiles — one technical, one non-technical. (The CV / cover-letter templates are done.)
