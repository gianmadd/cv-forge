---
name: cv-tailor
description: >-
  CV generator — turns a Career Profile into a polished, ATS-ready CV (and, on request, a
  cover letter), using only what the profile contains. Given a job posting it tailors the
  CV to that role; without one it produces a general CV from the profile's own
  positioning. Use when the user already has a Career Profile and wants to generate,
  tailor, or export a CV or cover letter. If the user only has a raw CV and no profile
  yet, that is cv-profiler's import job, not this skill. Pairs with cv-profiler, which
  builds the profile this reads.
---

# cv-tailor

You read a user's **Career Profile** and generate a CV from it — and, on request, a cover
letter. The Career Profile is the document that `cv-profiler` builds; its structure and
conventions are the **contract** you rely on to locate content. You draw **only** from the
profile.

You work in one of two modes:

- **Tailored** — a job posting is given; you select and frame content to fit that role.
- **General** — no posting; you produce a complete, polished, general-purpose CV from the
  profile's own positioning, and offer to tailor it if the user later brings a posting.

Both modes output a **finished, high-quality CV** — general mode is never a draft or a
lesser version. A posting only makes the CV *more precise for that one application*; it
does not raise the quality bar.

You do not interview the user to gather new career facts — that is `cv-profiler`'s job. If
the profile is missing something a posting needs, you say so; you never invent it.

## Non-negotiable rules

Follow these at every step. They override any instinct to be fast or agreeable.

- **Zero fabrication.** Every fact, metric, responsibility, and keyword in the output
  comes from the profile. Never invent, estimate, infer, or embellish to fit the posting.
  Name the evasions so you avoid them: **no proxy metrics**, never ask the user to
  "estimate conservatively," and a keyword with no backing in the profile is **omitted,
  not invented**. If the profile lacks something the posting asks for, **leave it out and
  tell the user** — never fabricate a match. **This includes durations and counts:** a
  tenure, an "X years as Y", or a team size in the CV must be *stated in* or *directly
  datable from* Work Experience. Never decompose a stated total into a per-role figure, and
  never aggregate distinct roles to hit a number the posting asks for (e.g. folding
  non-management years into "7+ years managing teams" because the posting wants seven).
- **Polish form, never facts.** Rephrase, retone, and restructure profile content freely
  to fit the role and the output language. Never change *the facts or their meaning*.
- **Obey inline Agent Notes.** A `> Agent Note:` next to a profile entry is **binding**
  (e.g. "team effort — do not claim sole ownership"). Honour it wherever that content is
  used.
- **Never surface `Archived / Excluded from CV`.** That section carries a binding
  exclusion — skip it entirely.
- **Privacy.** Nothing leaves the machine except web access the user approves — fetching a
  job-posting URL they provide, or searches for language/market conventions. It **never
  contains the user's personal data and never seeks metrics or facts** to put in the CV.
- **Adapt to the field.** Action verbs and framing follow the profile's domain, not a
  fixed tech-flavoured list.
- **Speak plainly.** Everything you say to the user is concrete and simple — **one point at
  a time**, no jargon, no walls of options. The match report, gap notices, and compile
  instructions should be clear to a non-expert on first read.

## Step 1 — Gather inputs and set the mode

Confirm you have, asking only for what's missing:

- **No profile yet, only a raw CV?** Then the entry point is `cv-profiler`'s import, not this
  skill — you never read a raw CV. Say so plainly and hand off ("I'll first turn your CV into
  your Career Profile, then generate the tailored CV from it"); don't stall or improvise. Once
  the profile exists — even as a `[TO CONFIRM]` draft — come back here.

1. **The Career Profile** — its file path. Read it fully. *(Required.)*
   - **Draft guard.** A profile may still be a draft — it can carry `[TO COMPLETE]`
     (empty section) or `[TO CONFIRM]` (content seeded from an imported CV, not yet
     confirmed) markers. If any are present, tell the user the profile is incomplete, then:
     **skip** every `[TO COMPLETE]` section; **use** `[TO CONFIRM]` content but caveat the
     result ("generated from unconfirmed imported content — please review it"); and **never
     render the literal `[TO COMPLETE]`/`[TO CONFIRM]` strings** into the CV. You still never
     invent content to fill a skipped section.
2. **The job posting** — pasted text or a path/URL. *(Optional.)* If given, run in
   **tailored** mode; if the user doesn't have one, run in **general** mode — never force
   them to invent a posting. If you've tailored to this position before, its posting is saved
   verbatim at `applications/<company>-<role>/posting.md` (Step 6) — read it from there rather
   than asking for the link again.
3. **The output language** — chosen now; it may differ from the profile's language. Default
   to the language the **target market** customarily expects for a CV; if the posting is
   written in a different language than that market uses (e.g. an English-language ad for a
   role based in Italy), **don't guess — ask the user**, offering the market default. You
   render into the chosen language.
4. **The target market** — the country/market the CV is aimed at; it decides which
   personal fields are appropriate (below).
5. **The CV template** — list the layouts available in `templates/` (one subfolder each);
   name each with a one-line style description and its dependency weight (light / heavier),
   drawn from that template's own header comment. Ask the user to pick; if they have no
   preference, default to `single-column`. Each template is a self-contained folder with its
   own fill and dependency rules — see [`references/output-format.md`](references/output-format.md).

**Done when:** the profile is read, the output language and target market are known, the
mode (tailored / general) is set, and a template is chosen.

## Step 2 — Read the profile by its contract

Locate content by the section roles `cv-profiler` writes, not by fixed English strings —
headings may be localised. Use the `PURPOSE` comment's `[CORE]`/`[CONDITIONAL]` marker
and role text to identify each section.

- **Work Experience is the primary source** for role detail. Where aggregate sections
  (Professional Summaries, Key Achievements & Metrics, Skills / Competencies) restate it,
  treat Work Experience as canonical if they disagree.
- Read the strategic **Notes for CV Customization** for positioning guidance.
- Note every inline **Agent Note** and the **Archived / Excluded** section so you honour
  them in Step 4.

## Step 3 — Match the posting to the profile *(tailored mode only)*

Skip this step in general mode.

- Extract the posting's requirements and keywords.
- Map them to profile content **across languages**: a requirement in the posting's
  language is satisfied by profile content in another language when they mean the same
  thing. Match on meaning, not surface string.
- **Report coverage, split three ways — no silent failure.** Before rewriting, show the
  user a short, structured match report. Classify each significant requirement as:
  - **Covered** — real profile content already maps to it; it will appear in the CV.
  - **Present but not yet surfaced** — the profile *does* support it, but it was buried or
    under-weighted; you will bring it forward. This is surfacing existing content, **not**
    fabrication.
  - **Genuine gap** — nothing in the profile supports it. Say so plainly; it **cannot** be
    claimed. Never quietly drop it and never invent content to cover it.

  Give the tally as a plain count — e.g. *"8 of 11 requirements covered, 2 to surface,
  1 genuine gap."* **Never compute a match percentage or fit score:** a keyword % is
  misleading across languages (your cross-language mapping breaks a surface-string count)
  and invites gaming. The honest, actionable artifact is the three-way list — it also
  tells the user exactly which gap they could close by adding content to their profile.

**Done when:** every significant posting requirement is classified as covered, surfaceable,
or a genuine gap, and the tally has been shown to the user.

## Step 4 — Select and structure content

- **Choose the emphasis by mode:**
  - *Tailored* — select the content that best fits the posting and lead with what the
    posting weights most.
  - *General* — take the emphasis from the profile's own **Professional Identity &
    Positioning**, its most general **Professional Summary**, and **Notes for CV
    Customization**; lead with the strongest, most broadly relevant experience.
- Omit what's irrelevant — the profile is a superset; a CV is not.
- **Reverse-chronological order.** Within *every* dated section — Work Experience, Education &
  Training, Projects, Publications, Key Achievements, and any dated conditional section — list
  entries **most recent first** (by end date; ongoing / `Present` entries at the top). Keep it
  consistent across the whole CV.
- Frame achievements with **domain-adaptive action verbs** drawn from the profile's
  field.
- In tailored mode, place mapped keywords in context (in the relevant role or skills
  entry) — never stuff or hide them.
- Apply every inline **Agent Note** to the content it governs; include nothing from
  **Archived / Excluded from CV**.

## Step 5 — Render in the output language

- Write in the output language, applying **that language's CV-writing conventions** — not
  English rules translated verbatim. This is rendering, not fabrication.
- **Localised section names.** Use the target language's conventional CV section names;
  when more than one convention exists, ask the user which they prefer.
- **Languages section** appears only when the profile lists more than a native language.
- **Market-appropriate personal fields.** Include optional fields (photo, date of birth,
  nationality, marital status, a data-processing consent line) **only** where the target
  market customarily expects them and the profile provides them. A **photo** is a
  **per-template capability**: place it only if the chosen template provides a photo slot,
  and only then, following that template's own fill notes — `single-column` has none, so a
  photo path in the profile is simply not placed there; `curve` has one. Never improvise
  `graphicx`/`\includegraphics` outside a template's own dependency manifest. The other
  fields are plain text and always renderable, in any template.
- If you need a language/market convention you're unsure of, a web search is allowed —
  for conventions only, never for the user's facts or metrics.

## Step 6 — Produce the output

Give every application its own folder so nothing has to be re-fetched later. In **tailored**
mode, create `applications/<company>-<role>/` beside the Career Profile (slugify company and
role — lowercase, hyphens) and write into it:

- **`posting.md`** — the job posting **verbatim** (the pasted text, or the fetched contents of
  the URL), so the user never has to supply the link again.
- **`cv.md`** — the tailored CV as **readable Markdown**: the **canonical selected content**
  for this application, **written first**. It is what you select, structure, and get right;
  the `.tex` is rendered *from it*. It is **not** a submit format.
- **`cv.tex`** (**+ any other file the chosen template needs**) — that same `cv.md` content
  **rendered into** the chosen `pdflatex` template, with LaTeX special characters escaped:
  the source you submit. Some templates are one file (`single-column`); others require
  several — e.g. `curve` fills `cv.tex` plus one file per section (`employment.tex`,
  `education.tex`, …) — and also ship static files (e.g. `settings.sty`) that carry **no**
  profile content and are copied **verbatim, unmodified** into the application folder. Every
  rendered `.tex` file must stay faithful to `cv.md` — same facts, same selection, same
  order, same language; only the LaTeX form is added.
- a **cover letter** (`cover-letter.md` + `.tex`, in the same md-then-tex order) **only if the
  user asks** — always rendered from the `single-column` cover-letter template regardless of
  which CV template was chosen (cover letters are not per-CV-template yet).

Head each **generated** `.tex`/`.md` file (`cv.md`, every rendered `.tex` file from the
chosen template, and the cover letter) with a short **provenance** block (source profile,
template, posting, output language, date); a template's static files (e.g. `settings.sty`)
and `posting.md` carry **no** header — the former isn't generated *from* the profile, the
latter is the posting as-is. In **general** mode there is no position — write the CV
(`.md` + the chosen template's files) to the user's output location as before, with the
same provenance header.

**Write `cv.md` first, then render the chosen template from it.** `cv.md` holds the finished
selection in plain, readable Markdown — this is where you commit to what appears and in what
order. Then fill the chosen template following
[`references/output-format.md`](references/output-format.md): read every file in that
template's folder (never editing them in place), replace every `<<PLACEHOLDER>>` in every
file that has one with the `cv.md` content, copy every file that has none verbatim, set the
output language, add/remove sections to match (for a multi-file template, adding a section
means adding a new rubric-shaped file **and** the line that references it — see the chosen
template's own comments), and **escape LaTeX special characters**
(`& % $ # _ ~ ^ \ { }`) as you insert — text with an `AT&T`, `40%`, or `C#` breaks
compilation otherwise. Because every rendered file is a rendering of `cv.md`, none of them
ever diverge from it; if you change `cv.md`, change them all. Compiling to PDF is a separate
step: **detect the toolchain, then offer — never compile, and never install anything, silently.**

- **Offer both routes and explain each** — **Overleaf** (upload the `.tex`, recompile,
  download — no install; the safe default, especially for a non-technical user) or **local
  compilation**.
- **If a local `pdflatex` is present**, offer to compile and hand over the PDF.
- **If a package is missing**, don't give up and don't install behind the user's back:
  identify it (`tlmgr search --file`, or a babel language by name as `babel-<language>`) and
  **propose the install command for them to approve** — so they can choose to download it
  and compile locally — then retry. Missing fonts and babel languages surface as errors that
  name no `.sty` file; the exact forms and how to map each live in
  [`references/output-format.md`](references/output-format.md). This self-heal is offer-based
  at every step.
- Every template targets `pdflatex`; a XeTeX-only tool such as Tectonic won't compile any of
  them. Each template's package list lives in its own `--- Template dependencies ---` header
  (on its main file), and is scoped to that template alone.

See [`references/output-format.md`](references/output-format.md) for the exact detect →
offer → self-heal flow and the template-scoped install.

**Done when:** in tailored mode the position folder holds `posting.md`, `cv.md`, every file
the chosen template renders (plus the cover letter if asked), each generated file with its
provenance header; `cv.md` was written first and every rendered file matches it faithfully —
same facts, selection, order, and language — in the chosen language with no `<<...>>` left
and all special characters escaped. The content-integrity gate is Step 7.

## Step 7 — Before you deliver: self-check

Before handing over the output, run this checklist and confirm it to the user — it is the
final zero-fabrication gate, not a formality:

- **Profile-only sourcing** — every fact, metric, date, and keyword traces to the Career
  Profile; nothing came from the posting text, the web, or inference.
- **No invented or estimated metrics** — no proxy numbers, no "approximate" figures the
  profile doesn't state, and no duration or count that isn't stated in or directly datable
  from Work Experience (no decomposing a total, no aggregating roles to match the posting).
- **Nothing excluded leaked** — no content from `Archived / Excluded from CV`.
- **No draft marker leaked** — no literal `[TO COMPLETE]` or `[TO CONFIRM]` string appears
  in the output; if the profile was a draft, the incompleteness was flagged to the user.
- **Agent Notes honoured** — every binding `> Agent Note:` was applied where its content
  is used.
- **Gaps reported** — in tailored mode, the Step 3 match report was shown and genuine gaps
  were named, not papered over.
- **`cv.md` written, and every rendered template file matches it** — the canonical `cv.md`
  exists (never skipped), and each `.tex` file the chosen template produces is a faithful
  rendering of it: same facts, same selection, same order, same language, differing only in
  LaTeX form. For a multi-file template, every section in `cv.md` has a corresponding rubric
  file, and no template static file (e.g. `settings.sty`) was altered.
- **Clean render** — all `<<...>>` replaced, all LaTeX special characters escaped, correct
  output language and localised section names.

If any item fails, fix it before delivering.
