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
- **`cv-tailor` runs with or without a job posting.** With a posting it *tailors* (selects
  and frames content to fit the role); without one it produces a *general* CV from the
  profile's own positioning (Identity, default Summary, Notes for CV Customization).
  Both modes output a **finished, high-quality CV** — general mode is not a draft; the
  posting only makes the CV more precise for that one application. *Why:* people often
  want a solid, complete CV before they have a specific opening — forcing a posting would
  block an obvious use case.
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
- **No domain- or profile-specific template** (neutral labels, Projects made explicitly
  optional) — a plumber and a data analyst render into the same layout options, never a
  "trade CV" vs. a "tech CV." *Why:* the template structure is already largely neutral; the
  real output bias lived in verbs and the example. (This is orthogonal to the **user-chosen**
  aesthetic templates decided in §10 — that choice is the user's taste, never inferred from
  the profile's domain.)

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

- **Zero fabrication** is absolute: no invented, estimated, or embellished facts. The
  rule **names its evasions** so a model can't technically comply while drifting: no proxy
  metrics, never "estimate conservatively", and a keyword with no profile backing is
  omitted, not invented.
- **Rephrasing is allowed by default** for form (tone, syntax, grammar, completeness,
  clarity) — a useful builder improves how things read. The invariant is that
  rephrasing never changes the facts or their meaning. *Why:* the original rule
  ("never edit the user's words without permission") was too strong; what must be
  protected is the content, not the exact words.
- **Match transparency is a three-way report, never a score.** In tailored mode
  `cv-tailor` classifies each posting requirement as *covered* / *present but not yet
  surfaced* / *genuine gap*, with a plain count (e.g. "8 of 11 covered"). No match
  percentage or fit score is computed. *Why:* a keyword % assumes same-language surface
  matching, which is false under the cross-language mapping that differentiates us, and a
  number invites gaming. The classified list is the honest artifact and reframes "no
  silent failure" as an actionable split — surface content the user already has vs. a gap
  they alone can close by editing the profile.
- **A delivery-time self-check gate** (`cv-tailor` Step 7): before handing over, the skill
  confirms an explicit checklist (profile-only sourcing, no proxy metrics, nothing from
  Archived/Excluded leaked, Agent Notes honoured, gaps reported, clean render). *Why:* it
  operationalises zero-fabrication as a gate rather than scattered prose. We stay
  prompt-only by design (no deterministic linter, to keep the skills portable and
  runtime-free), so a confirmed checklist is the portable form of enforcement.

## 8. Verification

- **A lightweight eval on non-technical profiles:** 2–3 scripted personas (e.g. nurse,
  plumber, teacher) plus an acceptance checklist (no tech jargon leaking into probes, no
  forced empty core section, conditionals proposed by the rubric, Languages handled).
- **An end-to-end test** (`cv-profiler` → `cv-tailor` → PDF) on at least one technical
  and one non-technical profile: no section lost (Languages included), the PDF compiles,
  and the non-technical CV isn't tech-flavored. *Why:* the whole value is "does it
  really de-bias?" — that needs to be checked, not assumed.
- *This states the decided verification approach; what was actually run — and what it
  found — is recorded in [`roadmap.md`](roadmap.md) (§Done).*

## 9. Examples & few-shot

- **Four example Career Profiles ship as disclosed few-shot, chosen to span the range.**
  They live in `skills/cv-profiler/references/` and are pointed at from `cv-profiler`'s
  `SKILL.md`, loaded on demand: a **skilled tradesperson** (metric-light, overlapping
  employed/self-employed, an `Archived/Excluded` section), a **recent graduate**
  (early-career, thin experience, an inline `Agent Note` on an unowned outcome, a
  `Volunteer Work` conditional), an **experienced teacher** (non-technical, a documented
  career break, a `Professional Development` conditional), and a **data analyst**
  (technical, metric-rich, core-sections-only). *Why:* examples are the strongest bias
  signal in the whole system — a set skewed toward polished, quantified, senior white-collar
  careers would re-bias generation toward the exact profile the project de-biases *away*
  from. The four deliberately mix technical/non-technical, metric-rich/metric-light,
  senior/early-career, and every structural feature (conditionals, inline notes, career
  gaps, archived content, overlapping roles) so the few-shot teaches breadth, not a
  stereotype. Keeping them inside the skill is what lets the agent use them at runtime (the
  installer ships only the skill folder).

## 10. Repository, packaging & authorship

- **The repo uses a conventional skills-repository layout:** `skills/<name>/SKILL.md`,
  `docs/`, `CONTEXT.md`, `CHANGELOG.md`, and `package.json` (metadata/versioning, no
  `bin`). Skills use the **Skills feature (`SKILL.md`)**, not subagents in
  `.claude/agents/`.
- **Runtime assets live inside the skill folder.** The installer copies only
  `skills/<name>/`, so anything a skill reads while running ships with it: CV /
  cover-letter templates in `skills/cv-tailor/templates/`, and the worked example profiles
  (disclosed few-shot) in `skills/cv-profiler/references/`. There is no repo-root
  `templates/` or `examples/`. *Why:* a repo-root asset would simply not travel with the
  installed skill.
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
  step, and the user chooses how.** At the end `cv-tailor` presents the two routes and
  explains the procedure for each: **Overleaf** (cloud, zero local install — the universal
  default, and the only realistic route for a non-technical user) or **local
  compilation**. If a local `pdflatex` is already present, it *offers* to compile and hand
  over the finished PDF directly — it does not compile silently. *Why:* the skill runs on the user's machine and must not force a heavy TeX
  install; the source it emits compiles anywhere, Overleaf included, and the audience is
  often non-technical, so the choice is offered with instructions rather than assumed.
- **Each tailored run persists in its own application folder.** `cv-tailor` writes per
  position to `applications/<company>-<role>/` beside the profile: the posting saved
  **verbatim** (`posting.md`, so the link is never needed twice — on a re-run it is read back
  from there), the CV as a readable `cv.md` **and** the `cv.tex` submit source, plus a
  provenance header (profile / template / posting / language / date) on each generated file.
  *Why:* persistence makes "regenerate" and "tweak this one" zero-question and keeps
  applications auditable, while the Career Profile stays the single source of truth — these
  are derivatives, not a second profile. **General mode also gets its own folder,
  `general/`, beside the profile** (singular — one general CV, not one per position),
  **revising** the original choice here of no folder / "the user's output location" — see §12
  for why (the iterate dispatch needs a stable place to check for an existing general CV, and
  the original choice was quietly producing loose files at the profile's location).
- **Within an application, `cv.md` is the canonical content and `cv.tex` is rendered from
  it — not two parallel siblings of the profile.** `cv-tailor` writes `cv.md` **first**
  (the finished selection in plain Markdown: what appears, in what order), then renders
  `cv.tex` from it by filling the template and applying LaTeX escaping. The `.tex` must stay
  faithful to `cv.md` — same facts, selection, order, and language, differing only in LaTeX
  form. *Why:* (1) it matches the mental model that the Markdown is the source of truth the
  `.tex` derives from, giving a reviewable content artifact before the LaTeX mechanics; and
  (2) it fixes a reliability bug — when the `.tex` was written first and `cv.md` was a "then,
  if there's time" afterthought subordinated at every step, the running skill routinely
  skipped `cv.md` entirely (nothing in Step 7 checked it existed). Writing `cv.md` first makes
  it exist **by construction**, and Step 7 now verifies both that it exists and that `cv.tex`
  matches it. This does **not** reopen "Markdown output excluded" below: that excludes `.md`
  as a *submit* format; `cv.md` here is the canonical local content, never the artifact you
  send — the `.tex`/PDF is.
- **Local compilation stays minimal, and its dependencies are scoped to the chosen
  template.** What a local install must provide is exactly the current template's package
  set, and the **authoritative list is the template's own `--- Template dependencies ---`
  manifest** (maintained alongside its `\usepackage` lines) — today the fonts
  `CormorantGaramond`/`charter` (plus `cormorantgaramond`'s non-auto-pulled transitive deps
  `fontaxes`/`mweights`/`xkeyval`), `fontawesome5`, the layout packages
  `titlesec`/`enumitem`/`tools`/`geometry`/`xcolor`/`microtype`/`hyperref`/`latexmk` (this
  set is the `resume.tex` manifest; the cover-letter template additionally needs
  `ragged2e`), and the output language's `babel`/hyphenation — and nothing beyond each
  template's own manifest. (`textcomp` is
  **not** on the list: it ships in LaTeX base, always present, nothing to install.) The
  template is compiled with
  **`pdflatex`** (it uses pdfTeX-only primitives, `\input{glyphtounicode}` +
  `\pdfgentounicode=1`, for an ATS-extractable text layer), so the local distribution must
  provide `pdflatex`. The recommended light route is **TinyTeX** (a minimal TeX Live that
  still ships `pdflatex`, with `tlmgr` to add only the packages the template needs); a bare
  `texlive-latex-base` is **not** enough because of the template's fonts and icons, and
  **Tectonic is off-target** because it runs the XeTeX engine, under which those pdfTeX
  primitives are undefined. When a different template with different dependencies is later
  chosen (a roadmap item), that template carries its own install note — the requirement
  follows the template, never a fixed global TeX install.
  - **Mechanism (implemented and verified).** Each template *declares its own dependency
    manifest* (a `--- Template dependencies ---` header in the `.tex`, in `tlmgr` package
    names), and `cv-tailor` runs a **detect → offer → self-heal** flow: detect
    `pdflatex`/`tlmgr`; offer both routes (and offer to set up TinyTeX when no `pdflatex`);
    and on a missing dependency, map the file to its package with `tlmgr search --file`,
    **propose** the `tlmgr install` for the user to approve (never silent), and retry. The
    manifest is the fast path; the self-heal is the backstop that covers transitive-dep gaps
    (`cormorantgaramond` does not auto-pull `fontaxes`) and all three error forms — missing
    `.sty`, missing font metric (`Metric (TFM) file not found`), and a missing `babel`
    language (`Unknown option '<language>'`, which names no file, mapped by name to
    `babel-<language>`).
- **Deliverable formats: PDF for v1; DOCX planned post-v1; `.tex` is the source.** PDF is
  the primary submit format (via compile) and the **only** format v1 ships. **DOCX** is
  planned for the ATS/recruiter cases that want Word, but is **explicitly out of v1** — it is
  a separate build (rendering path undecided: Pandoc-from-`.tex` vs a native `.docx` template,
  with per-format escaping distinct from LaTeX) and would delay publishing without
  strengthening the core. The README does not promise DOCX, so shipping PDF-only breaks no
  public commitment. **Markdown output is excluded** — the Career Profile is already Markdown
  and no one submits a `.md` CV. *Why:* PDF covers the central "ATS-readable submit" promise
  now; DOCX is real but niche and can follow.
- **The v1 Career Profile contract is frozen as-is.** The published v1 commits to the current
  structure: the **binary visibility model** (normal vs `Archived / Excluded from CV`), the
  **nine core sections** + the conditional set, and the `PURPOSE`-marker / inline-`Agent
  Note` conventions. A **richer visibility taxonomy** (always / variant-specific / on-request
  / reference-only, enabling named CV variants) is deliberately **post-v1**: its real trigger
  is named CV variants, still deferred (multi-template itself has since shipped — see below —
  without needing this taxonomy), and it would burden the interview with per-item tagging.
  *Why it's low-risk to freeze:* the structure is proven (both skills
  read/write it; verified end-to-end and across all three dispatch modes), and future
  evolution is **non-destructively migratable** on Re-Run via the alias map in
  `references/re-run.md` — so a later taxonomy is an additive, migratable change, not a break
  for existing profiles.
- **Licensed under MIT** (© 2026 Gian Marco Addati).
- **ATS-readability is a goal, owned at the template level.** The product's promise is
  CVs that automated screening tools parse cleanly. `cv-tailor` renders faithfully into
  the template; ensuring the template is itself AI-/ATS-readable is a property of the
  template, verified when it's added — not a constraint `cv-tailor` re-derives.
- **A photo is a per-template capability** — like dependencies, whether a CV can carry a
  photo is a property of the chosen template, not a universal feature. `cv-profiler` may
  still capture a photo path (the profile is a superset), but `cv-tailor` places a photo
  only when the template provides a slot for one. The original single-column template is
  deliberately **photo-less**; `curve` (below) is the first photo-bearing layout. *Why:*
  baking a photo slot into a template the many markets that reject photos would then have to
  strip is worse than adding it to a template built for photo-expecting markets.
- **Multiple templates, one per subfolder of `templates/`, each self-contained.** `cv-tailor`
  now offers a choice of CV layout (`single-column`, `curve`) instead of one fixed template;
  the user picks in Step 1, defaulting to `single-column`. Each template is its own folder
  under `skills/cv-tailor/templates/` carrying everything it needs: its main file (still
  read for its `--- Template dependencies ---` manifest, per the mechanism above), any
  supporting files, and — for a template built on a class with structural requirements of
  its own — files that exist purely to satisfy that class. *Why a folder, not another flat
  file:* the fill mechanism generalizes cleanly (below) without the two templates' files
  ever colliding by name, and it's exactly the "dependencies travel with the template"
  principle already decided above, now applied to the files themselves.
  - **The single-file fill assumption generalizes to "every file in the folder."**
    `cv-tailor` fills every file in the chosen template's folder that contains a
    `<<PLACEHOLDER>>`, and copies verbatim every file that doesn't (a static style file).
    *Why it had to generalize:* `curve` — built on Didier Verna's `CurVe` LaTeX class — has a
    hard structural constraint (via the `LTXtable` package it depends on) that **each CV
    section must live in its own file**; there is no single-file way to author it idiomatically.
    A template can still be one file (`single-column` still is); the contract is now
    "fill what has placeholders, copy what doesn't," which subsumes the one-file case rather
    than replacing it.
  - **The CurVe import was adapted, not ported verbatim, to stay inside the existing
    architecture.** Three deliberate simplifications from the upstream Overleaf template
    ("A Customised CurVe CV" by LianTze Lim, CC-BY-4.0, itself built on Verna's class):
    no `biblatex`/`biber` for the publications list (rendered as a plain rubric instead,
    like Experience) — *why:* a bibliography engine is a second build tool beyond `pdflatex`,
    for a section few profiles use; no `simpleicons` X/Twitter icon — *why:* one more package
    for a field the Career Profile's Contact Information doesn't even collect; and the cover
    letter stays `single-column`-only regardless of which CV template is chosen — *why:*
    authoring and maintaining a matching cover letter per CV template doubles that work for
    every future template, for a document whose visual match to the CV is a nice-to-have, not
    a requirement. All three are revisitable if a real need shows up.
  - **The decorative entry-prefix glyph (upstream default: a bookmark icon before every
    entry) is dropped, not carried over.** It added visual noise and, because a FontAwesome
    icon has no real Unicode mapping, a stray character in every entry's *extracted* text —
    the opposite of the ATS-readability this project holds itself to. *Why this one wasn't
    kept as a "later" option like the others:* it's pure decoration with an ATS cost and no
    corresponding content value, unlike publications support or the X icon, which are content
    trade-offs.

## 11. Import & review (post-v1 feature)

Letting `cv-profiler` **import an existing CV** and **review** it — turning the two
commonest starting points ("adapt my existing CV to a posting", "revise my CV") into
first-class on-ramps of the pipeline. Design decisions (grilled, and audited by three
independent lenses — boundary/ownership, user mental-model, contract integrity):

- **Import & review stay inside `cv-profiler` — no third skill.** They are on-ramps and
  behaviours, not a new tool: import *produces a Career Profile* (the same artifact New
  Build produces) and review *is* the existing gap-noticing muscle pointed at imported
  content. The decomposition test (a review-only skill earns its own description only if
  its trigger *and* output diverge) fails once you notice that splitting review would, by
  the same logic, force splitting import/resume/re-run too — they are all facets of one
  intent ("get my career into a profile"). *Why:* two skills, two verbs stays legible to
  the user; the Career Profile remains the single contract.
- **A distinct `[TO CONFIRM]` marker for extracted-but-unconfirmed content** (not a reuse
  of `[TO COMPLETE]`). Three section states now exist — empty, extracted-unconfirmed,
  confirmed — and the middle one needs its own token. *Why a new marker beats reusing
  `[TO COMPLETE]`:* reuse would force the "confirm vs fill" distinction to be *inferred*
  from whether content sits under the marker (fragile), would contradict the written rule
  "remove `[TO COMPLETE]` once it holds real content", and — decisively — would collide
  with `cv-tailor`'s draft guard, which must **skip empty sections but use extracted
  content**: one token cannot mean both "skip" and "use". A distinct marker makes both
  behaviours clean and keeps `[TO COMPLETE]`'s meaning unchanged. Both markers are now
  **shared contract tokens** documented in `career-profile.md` and read by both skills.
- **`cv-tailor` gains a draft guard.** It may be handed a still-incomplete profile (import
  → generate immediately, the "adapt an existing CV" path). It skips `[TO COMPLETE]`
  sections, uses `[TO CONFIRM]` content with an "unconfirmed — review it" caveat, and never
  renders either literal marker into the CV. *Why:* this makes the fast import→generate
  path work, and closes a **latent existing bug** — before the guard, a drafted profile fed
  to `cv-tailor` would leak the literal `[TO COMPLETE]` string into the output. This is a
  real (small) `cv-tailor` change, so the marker convention is mirrored into
  `career-profile.md` per the keep-in-sync invariant.
- **One review muscle, two call sites.** "Review" is content-gap-noticing (weak/unquantified
  bullets, vague positioning, a skill not evidenced, profile-internal consistency), defined
  once in `references/review.md` and invoked both after an import and as an on-request Re-Run
  **audit** of an existing profile. *Why one definition:* two copies would drift. *Why
  on-request for the audit:* a proactive critique on every Re-Run would break Re-Run's "you
  tell me what to change" character and the probe-once/user-authority rules.
- **Consistency vs formatting — a deliberate split.** *Profile-internal consistency* (dates,
  language-level scale, metric units) is the review muscle's job in `cv-profiler`;
  *CV-document formatting* (ATS-readability, layout, parseability) stays owned by `cv-tailor`
  alone. The word "format" was covering two different things; they are separated so neither
  skill critiques what it can't see. A Career Profile is not a CV, so it has no document-layout
  to critique — the honest answer to "is my layout ATS-friendly?" is to generate a CV.
- **Import is a dispatch on-ramp, offered not grabbed.** Two sub-flows: **Import → New**
  (no profile → seed a new one) and **Import → Enrich** (existing profile → reconcile in,
  under Re-Run). Dispatch stays file-based for the *mode*; a light pre-step notices a CV in
  the directory and makes **one soft offer** — never auto-extracting a stray file. The flow
  is always extract → seed → review → interview, so review-only is just an early stop, not a
  separate mode. `cv-profiler` becomes the **only raw-CV reader** (one-time source);
  `cv-tailor` still reads only the profile, so the single-contract model holds.
- **Extraction prefers a verbatim text layer over model vision.** `pdftotext -layout` /
  `pandoc` are more faithful than re-typing a document by sight (which can hallucinate).
  Dependencies use the same **detect → offer → propose-install** flow as `cv-tailor`'s
  `tlmgr` (never silent), with a gentle native-read fallback. Scanned/no-text-layer PDFs are
  read by sight **best-effort, with an explicit flag**; all tooling stays **local** (no cloud
  OCR — privacy). Extraction is verbatim and the duration/count guard applies.
- **GAP — format critique of an *external* CV has no owner (decided: don't build it).**
  `cv-profiler` reviews content only; `cv-tailor` won't read a raw CV. Rather than reopen
  that invariant, the product answer is *"import it and we generate an ATS-correct CV"* — we
  replace the layout, we don't critique it. **Watch:** a real demand for external-CV format
  critique is the trigger to revisit this (a Deferred item in `roadmap.md`).
- **GAP — anti-nag memory (decided: no dedicated state file).** The soft offer's outcome
  (imported / declined) is recorded in the provenance HTML comment *once a profile exists*.
  Before a profile exists there's nowhere to record it and re-offering next session is
  acceptable — not worth a dotfile (clutter, fragile across agents, a second state store).

## 12. Iteration & length targets

Two roadmap items, both entirely inside `cv-tailor` — no `cv-profiler` change, no Career
Profile contract change:

- **Regenerate vs iterate is a Step 1 dispatch, mirroring `cv-profiler`'s three-way
  pattern.** Finding an existing `applications/<company>-<role>/cv.md` (tailored mode) or
  `general/cv.md` (general mode), `cv-tailor` offers a choice — regenerate from scratch, or
  iterate on what's there — skipping the offer if the user already said which. *Why offered,
  not inferred:* consistent with the detect → offer discipline already used everywhere else
  in this skill (compiling, self-heal); the skill never silently picks a path for the user.
- **An iteration may pull new content from the profile, not just reword `cv.md`.** The
  profile is the superset `cv.md` was originally selected from, so "add my 2019
  certification" or "lead with leadership" are legitimate iterations, not out of scope. *Why:*
  restricting iteration to text-level edits would make it a worse editor than just opening
  the file by hand; the value is re-selecting from the same source of truth without
  re-running the whole skill.
- **Overwrite, not versioning.** An iteration overwrites `cv.md`, every rendered template
  file, and the cover letter in place — no history file, no `cv-v2.md`. *Why:* matches the
  "no structured data layer, no extra state" baseline that runs through this whole document;
  a persistent history is the *corrections log* idea in `roadmap.md` (deferred, and a
  different, larger commitment), not a side effect of this feature.
- **Every iteration re-runs Step 3's match report (tailored mode) and Step 7's self-check.**
  A dropped role or requirement must never silently turn "covered" into a gap. *Why:* the
  self-check alone verifies fidelity/fabrication, not coverage — only re-running Step 3
  catches a regression the self-check wouldn't.
- **Iteration scope excludes template, output language, and tailored/general mode.**
  Changing any of those is a full regenerate (a fresh Step 1), not an iteration. *Why:* they
  change the rendering mechanics themselves (placeholders, escaping, file structure), not the
  selected content — conflating them with content tweaks would turn "iterate" into a second,
  redundant generation path.
- **Length targets are asked explicitly in Step 1** — one page, two pages, or long-form (no
  constraint) — not inferred or left implicit. *Why:* it's common enough (academic CVs
  especially) to earn a direct question rather than only reacting to it after the fact via
  iteration.
- **Length is verified by compiling when possible, not just estimated.** With a local
  `pdflatex` available, `cv-tailor` reads the real page count straight from the compile log
  (`Output written on cv.pdf (N pages, ...)`) — no new dependency — and trims/recompiles in a
  bounded loop until it fits. Without a local compile (Overleaf only), it falls back to a
  per-template heuristic and says plainly that the count is an estimate, not verified. *Why:*
  a page count is a fact about the compiled PDF, not the Markdown selection; guessing and
  presenting it as fact would be a quiet accuracy regression, and Overleaf-only users
  shouldn't be blocked, just told the honest limit of what can be checked without a local
  toolchain.
- **Coverage wins over length.** If hitting a length target would cut content the match
  report marked "covered," `cv-tailor` stops, names what would be lost, and asks before
  cutting — a page constraint never silently downgrades a covered requirement. *Why:*
  consistent with the "no silent failure" principle behind the match report itself (§7) — an
  aesthetic constraint shouldn't be allowed to quietly undo a transparency guarantee.
- **General mode gains its own `general/` folder, revising §10's original "no folder"
  choice.** A single folder beside the profile (not one per position, since general mode has
  one CV), holding the same file set as an application folder minus `posting.md`. *Why now:*
  the original "no folder" choice pre-dated any need to locate a "current" general CV; the
  iterate dispatch above needs exactly that, and the alternative — writing loose files
  straight into the profile's working directory — is also just a worse outcome on its own
  (surfaced directly by a user testing the iterate/length-target changes).
- **Every run's LaTeX inputs and compile byproducts live in their own `tex/` subfolder, not
  loose beside `cv.md`.** `cv.tex` (+ any other file a multi-file template needs, e.g.
  `curve`'s per-section files and `settings.sty`), the compiled PDF, and any transient
  `.aux`/`.log`/`.out` file all go in `tex/`; only the human-readable `cv.md` (and
  `posting.md`/`cover-letter.md` in tailored mode) sit at the top level of the application/
  `general/` folder. Cleaning the compile's transient files after every local compile —
  success or failure — is now stated as **mandatory**, not an "or" alternative to leaving
  them. *Why:* a multi-file template (`curve` alone writes six-plus files) made the flat
  layout hard to scan, and stray `.aux`/`.log`/`.out` files were observed lingering after a
  real compile; grouping everything LaTeX into one subfolder fixes both at once without
  touching what's canonical (`cv.md` stays exactly where it was).
