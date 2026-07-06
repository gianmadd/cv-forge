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
- **`cv-profiler` written** (`skills/cv-profiler/SKILL.md` + `references/`): dispatch, Phase 0 calibration, core/conditional structure with `PURPOSE` markers, probe banks, gap noticing, conditional-proposal rubric, incremental saving, zero-fabrication, and the eval-driven hardening (aggregate-derivation guard, target-title scope, language-scale, user-authority clarification).
- **`cv-tailor` written** (`skills/cv-tailor/SKILL.md` + `references/`): reads the profile by its contract, cross-language keyword matching, domain-adaptive selection, localized rendering, market fields, three-way match report, delivery-time self-check, and the duration/count guard mirrored from `cv-profiler`.
- **CV / cover-letter templates written** (`skills/cv-tailor/templates/`): single-column `pdflatex`, ATS-oriented (`glyphtounicode`), each carrying its own dependency manifest; preview verified on Overleaf.
- **Four range-spanning example Career Profiles written**, wired into `cv-profiler` as disclosed few-shot (`skills/cv-profiler/references/`): a skilled tradesperson (metric-light), a recent graduate (early-career), an experienced teacher (non-technical, with a career break), and a data analyst (technical, metric-rich, core-only). Deliberately mixes technical/non-technical, metric-rich/metric-light, senior/early-career, and every structural feature so the few-shot doesn't bias toward polished white-collar careers. See [`decisions.md`](decisions.md) §9.
- **Local-compile detect → offer → self-heal wired into `cv-tailor`**: each template **owns its dependency manifest** (a `--- Template dependencies ---` header in the `.tex`); `cv-tailor` detects `pdflatex`/`tlmgr`, offers both routes (Overleaf / local), and self-heals missing packages by **proposing the `tlmgr install` command for approval** (never installs silently) — the backstop for transitive-dep gaps like `cormorantgaramond`'s `fontaxes`/`mweights`/`xkeyval`, covering the `.sty`, font-metric, and `babel`-language error forms.
- **Verification complete (both skills).** Two `cv-tailor` end-to-ends — an AI-engineer
  profile → tailored CV (surfaced the LaTeX-escaping fix) and an Italian literature-professor
  profile → PDF via TinyTeX (surfaced the currency-symbol fix; confirmed de-biasing on a
  humanities profile). An independently-graded **scripted persona interview eval of
  `cv-profiler`** caught a real fabrication — a stated 9-year total silently decomposed into an
  invented "~6 years as manager" — which **drove the aggregate-derivation guard** (plus
  target-title scope, language-scale, honest-phrasing, and pertinence-trigger fixes, and the
  user-authority clarification); a graded re-run confirmed the guard holds. The duration/count
  guard was **mirrored into `cv-tailor`**, and the consistency pass refined to never silently
  recompute a *user-stated* figure. All three **dispatch modes** verified PASS (New Build,
  Resume Draft, Re-Run — non-destructive old-format migration via the alias map). Finally, an
  **installed-skill walk** (skill symlinked read-only under `~/.agents/skills/`, profile/output
  in a separate writable dir) confirmed the template resolves via the installed path and a full
  fill compiles to a clean 1-page A4 PDF with a real ATS text layer; it drove two operability
  fixes now in `cv-tailor` — fresh-file fill (never `cp`-then-edit, which would inherit a
  read-only template's mode) and the `babel`-language self-heal form. See [`CHANGELOG.md`](../CHANGELOG.md).
- **Pre-publish cleanup done.** `CHANGELOG.md` restructured into standard sections (Added /
  Changed / Decided) and the verbose §Done condensed — checked against factual loss, with the
  verification record consolidated here and a stale cross-reference (`decisions.md` §8) repaired.
  Confirmed the shipped skills carry no dev-time leaks and stay consistent with `docs/`.
- **v1.0.0 published.** Committed and pushed to `main`, tagged `v1.0.0` with a GitHub
  release (notes from the `CHANGELOG`). README reworked for launch — an **app-flow banner**
  hero (also set as the GitHub **social preview**), a **Preview** section with an example CV +
  cover letter from a fictional profile, badges, and a clearer problem → quickstart → pipeline
  structure, plus a Credits section. Repo **description and topics** set, and install from the
  published repo verified on Claude Code (`skills list` shows both skills). Confirmed the repo
  conforms to the `skills` layout and the third-party template credit (Michael Lustfield,
  CC-BY-4.0) is in order. Delivery decisions: **manual versioning** (hand-maintained
  `CHANGELOG.md`, no changesets/CI) and **`skills`-CLI-only distribution** (no
  `.claude-plugin/plugin.json`, per `decisions.md` §10). A shipped cover-letter template bug
  was also fixed in the process (the accent rule crossed the name; `\hrule` → `\rule`).
- **Import & review an existing CV (post-v1).** `cv-profiler` seeds a Career Profile from a
  CV the user already has, then reviews and interviews to confirm/fill — built as **on-ramps
  inside `cv-profiler`** (Import → New / Import → Enrich), **not a third skill**. Review is
  one shared content-gap-noticing muscle run after an import and as an on-request Re-Run
  **audit**; extraction is verbatim (`pdftotext`/`pandoc`, detect-offer-propose, scans
  best-effort+flag, local-only); seeded content is marked **`[TO CONFIRM]`** and `cv-tailor`
  gained a draft guard so an imported profile can be generated immediately. A **plain-
  communication** guideline was added to both skills. Verified end-to-end; merged via PR #1.
  See [`decisions.md`](decisions.md) §11. Optional post-merge follow-ups: a graded eval + an
  independent cold-agent run, and a `writing-great-skills` quality pass over both skills.

## To build

One optional, low-priority follow-up remains:

1. **Incremental checks on other agents.** Claude Code is the verified, first-class target;
   the shared installer also symlinks the skills into other agents (Gemini CLI, GitHub
   Copilot, OpenClaw, …). Run the full pipeline on each as capacity allows and fix any
   agent-specific quirks (skill discovery, invocation, multi-turn interview, file I/O), rather
   than assuming untested multi-agent support. Agents that only accept passive "rules" (no
   invokable skills) are best-effort — see [`architecture.md`](architecture.md).

## Deferred / open questions

- **Format/ATS critique of an _external_ CV (watch — deliberately not built).** Import &
  review deliberately scope review to *content*; a Career Profile has no document-layout to
  critique, and `cv-tailor` reads only the profile, so no one critiques an external CV's
  *format*. The chosen answer is *"import it and we generate an ATS-correct CV"*. **Revisit the
  invariant that `cv-tailor` reads only the Career Profile** (never a raw document — see
  [`architecture.md`](architecture.md)) only if a real use case demands format critique of a
  CV the user won't regenerate. See [`decisions.md`](decisions.md) §11.
- ~~**Broaden the example profiles' range.**~~ **Done** — the shipped few-shot is now four
  range-spanning profiles (tradesperson · recent graduate · teacher · data analyst). See
  §Done above and [`decisions.md`](decisions.md) §9.
- **Let the user choose among several templates.** Ship more than one CV layout and let
  the user pick — ideally pointing them to a gallery/link where they can browse options,
  then generating into the chosen one. For now there is a single template; this is the
  path to multiple. **Each template carries its own dependency/install note**, since its
  fonts and packages determine the minimal local install (see `decisions.md` §10) — a
  lighter-dependency layout (default fonts, no icon package) is one axis of choice worth
  offering.
- **More generation options.** Enhancements to `cv-tailor` output, once the core is
  stable:
  - *Iterate on a generated CV* — tweak an already-produced CV ("shorten to one page",
    "lead with leadership", "drop the 2016 role") without regenerating from scratch.
  - *Length / format targets* — one-page vs two-page vs a long-form academic CV
    (publications-heavy). Today there is no length control.
  - *Batch* — one profile + N postings → N tailored CVs for an active job search.
  - *Keyword-gap / improvement report* — `cv-tailor` now ships the base signal: a
    three-way match report (covered / present-but-not-surfaced / genuine gap), see
    `decisions.md` §7. What remains is the **coaching** extension: given a posting, tell
    the user *how* to strengthen the profile against each gap, not just that it exists.

- **Quality & transparency ideas (to discuss — grill pros/cons, concreteness, cost).**
  A shortlist surfaced while reviewing comparable tools; each is deliberately *not* built
  yet and wants a design pass before commitment.
  - *Anti-AI-fingerprint / humanisation pass (multilingual).* A **polish-only** quality
    gate so generated CVs don't read as machine-written: per-language banned-buzzword
    lists, structural anti-patterns (bullets not all opening the same way, capped
    em-dashes, varied sentence length), and believability checks ("credible at this
    seniority?"). Concretely a checklist step in `cv-tailor` that touches *form only, never
    facts*. **Tension/cost:** comparable tools' lists are English-only; the real work is
    curating per-language buzzword sets and reconciling with multilingual rendering — not a
    one-file edit. The most distinctive of these ideas (nothing does it multilingually),
    but the least cheap.
  - *Richer content-visibility taxonomy in the Career Profile.* Replace today's binary
    (normal vs `Archived / Excluded from CV`) with levels such as always /
    variant-specific / on-request / reference-only, enabling **named CV variants** (e.g. an
    academic vs an industry CV) from one profile. **Tension/cost:** this changes the Career
    Profile — the contract between the two skills — so it touches `cv-profiler`,
    `cv-tailor` and `career-profile.md` together, and risks burdening the interview with
    per-item tagging. Only worth it once named variants / multi-template land; note
    `Notes for CV Customization` already covers part of this in prose. **Decided post-v1:**
    the v1 contract is frozen on the binary model (see `decisions.md` §10); this taxonomy is
    a later, Re-Run-migratable change, not a v1 blocker.
  - *Corrections log + output lineage header.* A persistent "never re-introduce this error"
    ledger that survives regeneration (a fabrication ratchet), plus a provenance stamp on
    each generated CV (source profile / template / posting / version) so "tweak this one"
    is a zero-question op and applications are auditable. **Tension/cost:** small and
    principle-aligned; the open question is *where the log lives* — in the profile (touches
    the contract and `cv-profiler`) or as `cv-tailor`-side state. Pairs with *iterate on a
    generated CV* above.
  - *Optional multi-persona critique stage.* After generating, an optional pass that
    reviews the CV through several reader lenses (ATS / recruiter / HR / hiring manager /
    domain expert) and returns ranked, concrete fixes — coaching, not fabrication.
    **Tension/cost:** scope-expanding (adds a mode to `cv-tailor`), best run with fresh
    context; avoid any numeric /100 score for the same pseudo-precision reason the match
    report has no percentage.
- **Output & scope questions (to decide).**
  - *DOCX output* — **decided post-v1** (v1 is PDF-only; see `decisions.md` §10). `.tex` is
    the source, Markdown excluded. When built, decide the **rendering architecture** — a
    separate `.docx` template vs a Pandoc/library conversion — bearing in mind that
    **escaping is per-format** (the LaTeX `& % $ #` rule does not apply to DOCX).
  - *Repurposing the profile into non-CV outputs* — LinkedIn "About", a short bio, an
    elevator pitch. Scope decision: §1 deliberately targets CVs only; broadening is a
    conscious choice, not a default.
  - *Tailoring to a company (no posting)* — sits between general and tailored: aim at an
    organisation's domain/values without a specific opening.
- **Revisit the skill decomposition.** Check that two skills is still the right number as
  features land: that neither takes on too many responsibilities, and whether a more
  natural separation emerges. The load-bearing seam to preserve is the **Career Profile as
  the contract** between build and generate. Watch `cv-profiler` for accretion once
  *import & review* arrive (interview / import / review become distinct on-ramps); a
  review-only skill is the main candidate third seam if its trigger and output diverge
  enough to earn its own always-loaded description. Do not split `cv-tailor`'s
  tailored/general/cover-letter *modes* into separate skills — they share the same
  profile-reading and rendering machinery, so splitting would only duplicate it.
- **Other agents beyond Claude Code** — handled by the shared installer; only
  *testing* remains, after the skills exist.

## Content produced

The skills author `PURPOSE` texts and probes at runtime (the rules and probe banks live
in the skills). The repo assets are in place: the CV / cover-letter templates
(`skills/cv-tailor/templates/`) and the four range-spanning example Career Profiles
(`skills/cv-profiler/references/`).
