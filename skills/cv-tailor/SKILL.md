---
name: cv-tailor
description: >-
  CV generator — turns a Career Profile into a polished, ATS-ready CV (and, on request, a
  cover letter), using only what the profile contains. Given a job posting it tailors the
  CV to that role; without one it produces a general CV from the profile's own
  positioning. Use when the user has a Career Profile and wants to generate, tailor, or
  export a CV or cover letter. Reads the profile that cv-profiler builds.
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
  If the profile lacks something the posting asks for, **leave it out and tell the user** —
  never fabricate a match.
- **Polish form, never facts.** Rephrase, retone, and restructure profile content freely
  to fit the role and the output language. Never change *the facts or their meaning*.
- **Obey inline Agent Notes.** A `> Agent Note:` next to a profile entry is **binding**
  (e.g. "team effort — do not claim sole ownership"). Honour it wherever that content is
  used.
- **Never surface `Archived / Excluded from CV`.** That section carries a binding
  exclusion — skip it entirely.
- **Privacy.** Nothing leaves the machine except web searches the user approves. Searches
  are for language/market conventions only, **never contain personal data, and never seek
  metrics or facts** to put in the CV.
- **Adapt to the field.** Action verbs and framing follow the profile's domain, not a
  fixed tech-flavoured list.

## Step 1 — Gather inputs and set the mode

Confirm you have, asking only for what's missing:

1. **The Career Profile** — its file path. Read it fully. *(Required.)*
2. **The job posting** — pasted text or a path/URL. *(Optional.)* If given, run in
   **tailored** mode; if the user doesn't have one, run in **general** mode — never force
   them to invent a posting.
3. **The output language** — chosen now; it may differ from the profile's language. You
   render into this language.
4. **The target market** — the country/market the CV is aimed at; it decides which
   personal fields are appropriate (below).

**Done when:** the profile is read, the output language and target market are known, and
the mode (tailored / general) is set.

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
- **No silent failure.** Any posting keyword you cannot map to real profile content, list
  explicitly for the user — never quietly drop it and never invent content to cover it.

**Done when:** every significant posting requirement is either mapped to profile content
or reported as unmatched.

## Step 4 — Select and structure content

- **Choose the emphasis by mode:**
  - *Tailored* — select the content that best fits the posting and lead with what the
    posting weights most.
  - *General* — take the emphasis from the profile's own **Professional Identity &
    Positioning**, its most general **Professional Summary**, and **Notes for CV
    Customization**; lead with the strongest, most broadly relevant experience.
- Omit what's irrelevant — the profile is a superset; a CV is not.
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
  market customarily expects them and the profile provides them. A **photo** comes from
  the path/note in the profile and is placed at generation time.
- If you need a language/market convention you're unsure of, a web search is allowed —
  for conventions only, never for the user's facts or metrics.

## Step 6 — Produce the output

Fill the `pdflatex` template in `templates/` following
[`references/output-format.md`](references/output-format.md): replace every `<<PLACEHOLDER>>`,
set the output language, add/remove sections to match your selection, and **escape LaTeX
special characters** (`& % $ # _ ~ ^ \ { }`) in every value you insert — profile text with
an `AT&T`, `40%`, or `C#` breaks compilation otherwise. Produce a **cover letter only if
the user asks**. Compiling to PDF is a separate step — the user compiles on **Overleaf**
(no install) or with a local `pdflatex`; offer both.

**Done when:** the filled `.tex` is written in the chosen language, no `<<...>>` remains,
all special characters are escaped, every included fact traces to the profile, all Agent
Notes are honoured, and — in tailored mode — any unmatched posting requirements have been
reported to the user.
