# Changelog

All notable changes to `cv-forge` are documented here.

The format follows common changelog conventions, and the project aims to follow
semantic versioning.

## [Unreleased]

### Added
- Project scaffold and documentation set under `docs/` (architecture, Career Profile spec, principles, decisions, roadmap), plus `package.json` and `CONTEXT.md`.
- **`cv-profiler` skill** (`skills/cv-profiler/SKILL.md`) — the interview: three-way dispatch (New Build / Resume Draft / Re-Run), Phase 0 calibration, nine core sections + conditionals with `PURPOSE` markers, gap noticing, incremental saving, internationalisation, and zero-fabrication rules. Branch-specific reference (probe banks, Re-Run migration) disclosed under `references/`.
- **`cv-tailor` skill** (`skills/cv-tailor/SKILL.md`) — the generator: reads the profile by its contract, cross-language keyword matching with no silent failure, domain-adaptive selection, localised rendering, market-appropriate fields, and ATS-readable compilation (output-format reference under `references/`).
- **CV and cover-letter templates** (`skills/cv-tailor/templates/`) — single-column, `pdflatex`, ATS-oriented (`glyphtounicode`), with a placeholder fill contract. Each template carries its own `--- Template dependencies ---` manifest in `tlmgr` package names (including `cormorantgaramond`'s non-auto-pulled `fontaxes`/`mweights`/`xkeyval` transitive deps), so the dependency requirement travels with the template rather than a fixed global install.
- **Four range-spanning example Career Profiles** (`skills/cv-profiler/references/`), wired into `cv-profiler` as disclosed few-shot: a tradesperson (metric-light; overlapping employed/self-employed; `Archived/Excluded`), a recent graduate (early-career; inline `Agent Note`; `Volunteer Work`), a teacher (non-technical; documented career break), and a data analyst (technical; metric-rich; core-only). Deliberately mixes technical/non-technical, metric-rich/-light and senior/early-career so the few-shot doesn't bias generation toward polished, quantified, white-collar careers. See `docs/decisions.md` §9.
- **`cv-tailor` general mode** — with no job posting it produces a complete, high-quality general CV from the profile's own positioning; a job posting only sharpens it (tailored mode).
- **Match-transparency report** (`cv-tailor` Step 3) — each posting requirement classified *covered* / *present but not yet surfaced* / *genuine gap*, with a plain count and **no match percentage or fit score**. Reframes "no silent failure" as an actionable split. See `docs/decisions.md` §7.
- **Delivery-time self-check gate** (`cv-tailor` Step 7) — an explicit zero-fabrication checklist (profile-only sourcing, no proxy metrics, nothing from Archived/Excluded, Agent Notes honoured, gaps reported, clean render) confirmed before hand-over.
- **Local-compile detect → offer → self-heal** (`cv-tailor` Step 6 + `references/output-format.md`) — detects `pdflatex`/`tlmgr`, offers both routes (Overleaf default / local) and offers to set up TinyTeX when no `pdflatex` is found; never compiles or installs silently. On a missing dependency it identifies the package (`tlmgr search --file`) and **proposes** the `tlmgr install` for the user to approve, then retries. Recognises all three error forms: missing `.sty`, missing font metric (`Metric (TFM) file not found`), and missing `babel` language (`Unknown option '<language>'`, which names no file, mapped by name to `babel-<language>`).

### Changed
- **LaTeX rendering hardened for compilation** (`cv-tailor` SKILL.md + `references/output-format.md`) — special-character escaping (`& % $ # _ ~ ^ \ { }`), `~`-as-"approximately" → `$\sim$`, currency/non-ASCII symbol mapping (templates load `textcomp`; `€` → `\texteuro`, `£` → `\textsterling`, `±` → `$\pm$`, …), and `\urlstyle{same}`. Surfaced by the end-to-end tests.
- **Zero-fabrication rules sharpened, with a shared duration/count guard** — the rule names the evasions it must avoid (no proxy metrics, never "estimate conservatively", unbacked keywords omitted not invented). An **aggregate-derivation guard** in `cv-profiler` forbids any duration, count, or "X years as Y" the user didn't state or that isn't directly datable from Work Experience, never decomposes a stated total into a per-role figure, and adds a final consistency pass; the same guard is **mirrored into `cv-tailor`** (never aggregates distinct roles to hit a number the job posting asks for). Lifted to `docs/principles.md` as a shared rule.
- **Zero-fabrication vs. user authority clarified** (`cv-profiler` SKILL.md + `docs/principles.md`) — the rule constrains the *tool*, not the user; when the user asks to record something they themselves flagged as untrue, the tool offers honest framing **once**, then respects their decision — no lecture, no silent known-false claim.
- **`cv-profiler` interview refinements** — target-title scope stays within the roles/level the user named (broadening needs confirmation); language-scale capture keeps the user's own words (a CEFR grade beside "native"/"fluent" is not force-converted); asked-for-embellishment leads with honest phrasing, not a refusal; the conditional `PURPOSE` pertinence-trigger names the *kind* of profile generically. The Re-Run consistency pass never silently recomputes a user-stated figure — only tool-derived figures are auto-corrected.
- **Installed-skill operability** (`cv-tailor` SKILL.md + `references/output-format.md`) — the fill step writes the filled `.tex` as a **fresh file** in the user's output location, explicitly not `cp`-then-edit, because an installed skill's template can be read-only and a copied file would inherit that mode and fail to fill.

### Decided
- **v1 ships PDF-only; DOCX is post-v1** — a planned separate build (rendering path undecided). `.tex` is the source; Markdown output is excluded. The README promises no DOCX, so PDF-only breaks no public commitment.
- **v1 Career Profile contract frozen as-is** — the binary visibility model (normal vs `Archived / Excluded from CV`), the nine core sections + conditionals, and the `PURPOSE`/inline-`Agent Note` conventions. A richer visibility taxonomy is deliberately post-v1; freezing is low-risk because contract evolution is non-destructively migratable on Re-Run (alias map).
- **`SKILL.md` frontmatter is `name` + `description` only**; both skills are model-invoked and user-invokable; each `SKILL.md` is self-contained.
- **Compile hand-off** — `cv-tailor` offers both routes and explains each: Overleaf (zero-install default, realistic for non-technical users) or local compilation; when a local `pdflatex` is present it offers to compile rather than doing so silently. Local dependencies are scoped to the chosen template (TinyTeX as the light route).

See `docs/decisions.md` §10 for the full rationale behind these decisions.

### Notes
- Verification is complete — both skills exercised end-to-end (technical and non-technical profiles → CV → PDF), an independently-graded scripted persona interview eval of `cv-profiler`, all three dispatch modes, the local-compile self-heal, and an installed-skill walk. For what was run and found, see `docs/roadmap.md` (§Done).
