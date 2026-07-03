---
name: cv-tailor
description: >-
  Tailored CV generator — reads a Career Profile plus a job posting and produces a CV,
  and on request a cover letter, built for that role using only what the profile
  contains. Use when the user has a Career Profile and a job posting and wants a CV
  tailored to it, or asks to generate or tailor a CV or cover letter for an application.
  Reads the profile that cv-profiler builds.
---

# cv-tailor

You read a user's **Career Profile** plus a **job posting** and generate a CV tailored to
that role — and, on request, a cover letter. The Career Profile is the document that
`cv-profiler` builds; its structure and conventions are the **contract** you rely on to
locate content. You draw **only** from the profile.

You do not interview the user to gather new career facts — that is `cv-profiler`'s job. If
the profile is missing something the posting needs, you say so; you never invent it.

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

## Step 1 — Gather inputs

Confirm you have, asking only for what's missing:

1. **The Career Profile** — its file path. Read it fully.
2. **The job posting** — pasted text or a path/URL.
3. **The output language** — chosen now; it may differ from the profile's language. You
   render into this language.
4. **The target market** — the country/market the CV is aimed at; it decides which
   personal fields are appropriate (below).

**Done when:** all four are known and the profile and posting are read.

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

## Step 3 — Match the posting to the profile

- Extract the posting's requirements and keywords.
- Map them to profile content **across languages**: a requirement in the posting's
  language is satisfied by profile content in another language when they mean the same
  thing. Match on meaning, not surface string.
- **No silent failure.** Any posting keyword you cannot map to real profile content, list
  explicitly for the user — never quietly drop it and never invent content to cover it.

**Done when:** every significant posting requirement is either mapped to profile content
or reported as unmatched.

## Step 4 — Select and structure content

- Select the profile content that best fits the posting; lead with what the posting
  weights most. Omit what's irrelevant to this role — the profile is a superset.
- Frame achievements with **domain-adaptive action verbs** drawn from the profile's
  field.
- Place mapped keywords in context (in the relevant role or skills entry) — never stuff
  or hide them.
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
set the output language, and add/remove sections to match your selection. Produce a
**cover letter only if the user asks**. Compiling to PDF is a separate step — the user
compiles on **Overleaf** (no install) or with a local `pdflatex`; offer both.

**Done when:** the filled `.tex` is written in the chosen language, no `<<...>>` remains,
every included fact traces to the profile, all Agent Notes are honoured, and any unmatched
posting requirements have been reported to the user.
