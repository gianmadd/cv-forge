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
- **Two example Career Profiles written**, wired into `cv-profiler` as disclosed few-shot (`skills/cv-profiler/references/`): a senior AI/ML engineer (technical) and an experienced physician (non-technical).
- **Verification passed (both skills).** Two `cv-tailor` end-to-ends: AI-engineer profile →
  tailored CV (surfaced the LaTeX-escaping fix) and an Italian literature-professor profile →
  PDF via TinyTeX (surfaced the currency-symbol fix; confirmed de-biasing on a humanities
  profile). Plus a **scripted persona interview eval of `cv-profiler`** — two de-biasing-edge
  personas (Italian metric-light career-changer, English early-career graduate), each
  interviewed by an agent running the real skill and graded by an **independent** evaluator
  against a 24-item checklist with an objective fabrication trace. It caught a real
  fabrication (a stated 9-year total decomposed into an invented "~6 years as manager"),
  which drove the **aggregate-derivation guard** plus smaller hardening fixes (target-title
  scope, language-scale faithful capture, "lead with honest phrasing", pertinence-trigger
  wording) and the user-authority clarification; a graded **re-run confirmed the guard holds**
  (no recurrence, no new fabrication).
- **Local-compile detect-and-offer wired into `cv-tailor`** and the dependency-structuring question settled: each template **owns its dependency manifest** (a `--- Template dependencies ---` header in the `.tex`), and `cv-tailor` detects `pdflatex`/`tlmgr`, offers both routes, and **self-heals missing packages by offering the `tlmgr install` command for approval** (never installs silently) — the robust backstop for transitive-dep gaps like `cormorantgaramond`'s `fontaxes`/`mweights`/`xkeyval`.

## To build

*(Verification is complete — see **Done** above.)*

1. **Flow & operability check.** Walk the full pipeline as an installed skill and confirm
   it runs cleanly at the operational level. Fix whatever trips the flow.
   - **Done — compile hand-off.** The compile hand-off (Overleaf / local `pdflatex`) is
     wired and verified end-to-end, and the "helper scripts?" question is answered: the
     skills stay **script-free** (detect-and-offer + self-heal, no wrapper) — see the
     sub-bullets below.
   - **Remaining — installed-skill walk.** Still to confirm: file **permissions** for
     reading/writing the profile and the output when the skill runs as installed, and that
     the profile → `cv-tailor` hand-off is smooth from the user's real file locations (not
     just a scratch dir).
   - **Local compilation — decided and now exercised.** The compile hand-off is decided
     (see `decisions.md` §10): default to **Overleaf** (zero install), offer the local
     route with its steps, and when a local `pdflatex` is present *offer* to compile and
     hand over the PDF (never silently); the minimal install is scoped to the template,
     with **TinyTeX** as the light route (**not** Tectonic — it runs XeTeX, which can't
     compile the template's pdfTeX `glyphtounicode` primitives). The local path has now
     been run end to end: TinyTeX (~300 MB) + the template's `tlmgr` packages compiled the
     non-technical Italian profile to a clean 1-page PDF with `pdflatex`. **Done:** the
     **detect-and-offer** behaviour is now wired into `cv-tailor` (Step 6 +
     `references/output-format.md`) and **verified end-to-end** — a filled Italian
     `resume.tex` compiled to a clean 1-page A4 PDF (real ATS text layer), and a
     clean-machine simulation (7 template packages removed) drove the self-heal loop back to
     a clean PDF. That run fixed two things now in the skill: the manifest must use `tlmgr`
     package names (`tabularx`→`tools`, `textcomp` is base), and a missing *font* reports as
     `I can't find file '...'` / `Metric (TFM) file not found`, not `File 'X.sty' not
     found` — so self-heal catches both forms.
   - **How to structure local-compile dependencies — decided.** The hands-on TinyTeX run
     showed the install splits on two axes: **per-template** packages (fonts, icons,
     layout — this template needs `fontawesome5`, `cormorantgaramond` + its transitive deps
     `fontaxes`/`mweights`/`xkeyval`, `charter`, `titlesec`, `enumitem`, `microtype`) and
     **per-language** packages (`babel-<output-language>`, chosen at generation). A
     hardcoded package list is fragile — `cormorantgaramond`'s `fontaxes`/`mweights` deps
     were **not** auto-pulled and surfaced as compile errors. **Chosen (option b + c):**
     base TinyTeX for everyone + **each template owns its dependency manifest** (a
     `--- Template dependencies ---` header in the `.tex`, maintained with its
     `\usepackage` lines) that `cv-tailor` installs on offer, **plus the self-heal retry**
     as the backstop — on a missing dependency (`File 'x.sty' not found`, or the font form
     `I can't find file '...'` / `Metric (TFM) file not found`), `tlmgr search --file` →
     propose the `tlmgr install` for approval → retry. The manifest is the fast path (one
     install, captures transitive-dep gaps and tlmgr-name≠usepackage-name mismatches that
     runtime discovery misses); self-heal guarantees convergence even if the manifest
     drifts — verified: it caught the manifest's own tlmgr-name errors during the
     clean-machine run. This
     is script-free and consistent with "deps follow the template". (Note: a minimal
     TinyTeX warns on missing non-English hyphenation patterns; harmless with the current
     `\raggedright` template, but a justified template would want `fmtutil-sys --all` after
     adding a language.)
2. **Cleanup before publishing.** Tidy internal design notes so the deliverables are
   clean and consistent.
3. **Publish & make the repo presentable.** Confirm the repo conforms to the `skills`
   convention, then publish and verify installation on Claude Code — followed by
   incremental checks on other agents. Alongside publishing, do the usual repo-launch
   polish so it looks presentable:
   - **README aesthetics** — clearer structure, badges (license, install), a preview
     image of a generated CV, and a crisp install + usage walkthrough.
   - **Release** — tag a version, promote `[Unreleased]` in `CHANGELOG.md` to a numbered
     release, and cut a GitHub release.
   - **Repo metadata** — description, topics/tags, social preview image.
   - **Contributor basics** — check `CONTRIBUTING`/issue templates as needed; confirm
     `LICENSE` and any third-party template credit are in order.

## Deferred / open questions

- **Import & review an existing CV.** Let `cv-profiler` take a CV the user already has and
  do two things that share one entry point:
  1. **Review it** — a *disciplined* critique grounded in our principles: ATS-readability
     and structure, weak or unquantified bullets, inconsistencies (dates, formats),
     inflated claims, and (if a posting is given) coverage gaps against it. It **flags and
     asks**, never invents fixes — this is the existing gap-noticing muscle applied to an
     imported CV.
  2. **Import it** — extract the content (zero-fabrication) to seed a Career Profile, then
     interview to fill the gaps the review surfaced, and regenerate an improved CV.
  The natural flow: *upload CV → review/flag → interview to fix → build/enrich profile →
  regenerate*. This makes the two commonest cases work through the existing pipeline:
  *adapt an existing CV to a posting* (import → `cv-tailor` tailored) and *revise one
  without a posting* (import → `cv-tailor` general). **Evaluate first:** how the agent
  reads the uploaded document (PDF / DOCX / plain text) — whether it needs a helper script
  or a parsing tool, which formats to support, and how extraction/review stay
  zero-fabrication. Ties into the flow/operability helper-scripts question.
- **Broaden the example profiles' range.** The two shipped examples are both senior,
  credentialed, metrics-rich professionals. Add at least one that stresses the de-biasing
  edge the project targets — a trade / early-career / career-changer profile, deliberately
  metric-light and leaning on transferable skills — so the few-shot (and the eval) cover
  the harder case, not just polished white-collar careers. **Head start:** the persona eval
  already produced two such profiles (an Italian metric-light career-changer and an English
  early-career graduate); once reviewed and de-personalised they could seed shipped examples.
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
    `Notes for CV Customization` already covers part of this in prose.
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
  - *DOCX output* — the deliverable set is **PDF + DOCX**, with `.tex` as the source
    (Markdown excluded — decided). DOCX is not built yet: decide the **rendering
    architecture** — a separate `.docx` template vs a Pandoc/library conversion — bearing
    in mind that **escaping is per-format** (the LaTeX `& % $ #` rule does not apply to
    DOCX).
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
(`skills/cv-tailor/templates/`) and the two example Career Profiles
(`skills/cv-profiler/references/`). Broadening the examples' range is tracked under
*Deferred / open questions*.
