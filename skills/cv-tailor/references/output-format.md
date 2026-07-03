# Output format — filling and compiling the template

Consulted at the **compile** step. The template files live in
[`../templates/`](../templates/): `resume.tex` (the CV) and `cover-letter.tex` (produced
only on request). Both are single-column, **`pdflatex`**-based, and ATS-oriented.

## How to produce the output

1. **Copy the template** into the user's output location — do not edit the file in the
   skill folder. For a cover letter, also copy `cover-letter.tex`.
2. **Set the language.** Change the `babel` option (`\usepackage[english]{babel}`) to the
   output language: `italian`, `french`, `ngerman`, `spanish`, `portuguese`, …
3. **Replace every `<<PLACEHOLDER>>`** with content selected from the profile, in the
   output language. Leave no `<<...>>` behind. Add or remove `\entry` blocks, bullets, and
   whole `\section`s to match what you selected for this job.
4. **Do not compile silently in the dark.** Hand the user the filled `.tex` and either
   compile it (see below) or tell them how.

## Section mapping (profile → template)

| Template section | Filled from the profile |
| --- | --- |
| `\cvheader` name + role | Contact Information + Professional Identity & Positioning |
| `Summary` | the best-fitting Professional Summary angle for this posting |
| `Experience` (`\entry` + `bullets`) | Work Experience — the primary source; one entry per selected role, most recent first |
| `Education` | Education & Training |
| `Skills` (`tabularx`) | Skills / Competencies, filtered to the posting |
| `Languages` (commented out) | uncomment **only** if the profile lists more than a native language |

Honour every inline `> Agent Note:` on the content it governs, and include nothing from
`Archived / Excluded from CV`.

## Compiling to PDF

The output is a standard `pdflatex` document, so:

- **Overleaf (no local install):** upload the `.tex` (plus a photo if used) to a blank
  Overleaf project and compile — the menu's default `pdfLaTeX` works as-is.
- **Locally:** run `pdflatex` twice (`pdflatex resume.tex`) if a local TeX distribution is
  installed. Needs the `fontawesome5`, `cormorantgaramond`, and `charter` packages.

## Staying ATS-readable when filling

The template is built to parse cleanly; keep it that way while filling:

- Real text only — never bake text into an image. A **photo** goes in only where the
  target market expects one, from the path in the profile, and never carries text.
- Put mapped keywords in context (in the relevant role or skill line), never stuffed.
- Keep the single-column flow; don't add multi-column blocks, text boxes, or headers/
  footers that hold real content.

## Cover letter

Produced **only when the user asks**. Same fill rules, same output language, drawn from
the same selected content; address it to the posting's role and organisation.
