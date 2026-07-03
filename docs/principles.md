# Principles

Rules that both skills follow. These are cross-cutting; skill-specific behavior
lives in each `SKILL.md`, and the reasoning behind these choices is in
[`decisions.md`](decisions.md).

## Content integrity

- **Zero fabrication.** Every fact, metric, responsibility, and claim comes from the user. Nothing is invented, estimated, or embellished. If the user doesn't have a number, it's left out — never suggested.
- **Polished, never invented.** Wording is refined freely — tone, clarity, grammar, structure, completeness — because a useful builder improves how things read. What is protected is the *facts and their meaning*, never the exact words. Rephrasing must never change what the user actually claimed.
- **Consent for interpretation.** Binding per-entry Agent Notes are added only with the user's agreement, and are written so the user can read and understand them.

## Work-agnostic by design

- The interview and the output **adapt to the user's field** rather than forcing a single mould. A calibration step tunes tone, examples, and which sections are proposed.
- The Career Profile has a **fixed core** (present for everyone) plus **conditional sections** proposed only when genuinely relevant — so no one ends up with empty, irrelevant sections.
- Emphasis shifts for career changers (transferable skills, transition rationale) and early-career profiles (studies, projects, internships, volunteering) — without separate rigid branches.

## International, not US-centric

- No hardcoded US assumptions. GPA, security clearance, and similar fields are raised only when relevant to the user's context, never by default.
- De-biasing works **both ways**: as well as *not* forcing US-specific fields, the skills can *add* fields a target market expects (e.g. photo, date of birth, nationality, marital status in some countries; a data-processing consent line on CVs where it's customary). These are always optional and raised only when the target market warrants them.

## Multilingual

- The interview is conducted in the language the user writes in.
- The **Career Profile's language** is chosen explicitly and may differ from the conversation language.
- The **generated CV's language** is chosen at generation time; `cv-tailor` renders in that language, maps job-posting keywords across languages, and follows that language's CV-writing conventions.
- Skill prompts themselves are written in English for portability.

## Privacy

- The Career Profile contains personal information and stays **entirely on the user's machine**.
- Before storing sensitive information (e.g. health-related reasons for a career gap, immigration status), the skill asks for explicit consent and offers a neutral version or omission.
- **Nothing is sent externally** except explicit, transparent web searches — which never include the user's personal data.
- These protections do **not** depend on git or a `.gitignore`; they hold regardless of how (or whether) the user version-controls their files.
