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
4. **Escape LaTeX special characters in every value you insert.** Profile text routinely
   contains characters that break `pdflatex` or render wrong. Escape them as you fill:

   | Char | Write as | | Char | Write as |
   | --- | --- | --- | --- | --- |
   | `&` | `\&` | | `~` | `\textasciitilde{}` |
   | `%` | `\%` | | `^` | `\textasciicircum{}` |
   | `$` | `\$` | | `\` | `\textbackslash{}` |
   | `#` | `\#` | | `{` `}` | `\{` `\}` |
   | `_` | `\_` | | | |

   So `AT&T` → `AT\&T`, `Ben & Jerry's` → `Ben \& Jerry's`, `R&D` → `R\&D`, `40%` →
   `40\%`, `$1.2M` → `\$1.2M`, `#2` → `\#2`, `C#/.NET` → `C\#/.NET`. This applies to every
   field — names, employers, bullets, skills. A single unescaped `%` silently eats the
   rest of its line; an unescaped `&`/`$`/`#` aborts compilation.

   **The "approximately" trap.** When profile text uses `~` to mean *approximately*
   (e.g. "~5 years", "~$2M"), escaping it to `\textasciitilde{}` renders a raised literal
   tilde, which reads wrong. Use `$\sim$` instead: `~5 years` → `$\sim$5 years`. (A bare
   `~` in raw LaTeX is a non-breaking space, so it must never be left unescaped either.)

   **Currency and other non-ASCII symbols.** The table above covers the characters that
   *break* compilation, but common CV symbols beyond accented letters also need a macro —
   a bare `\euro` is undefined, and a literal glyph may not exist in the font. The template
   loads `textcomp`, so use: `€` → `\texteuro`, `£` → `\textsterling`, `¥` → `\textyen`,
   `¤` → `\textcurrency`; and for maths-like symbols `±` → `$\pm$`, `×` → `$\times$`,
   `№` → `No.`. So `90.000 €` → `90.000~\texteuro`. When unsure a symbol will render,
   fall back to the word ("euro").
5. **Do not compile silently in the dark.** Hand the user the filled `.tex` and either
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

Compiling is a separate step and the user chooses how — **offer both routes and explain
each**, then, if a local toolchain is present, offer to run it for them (never compile
silently).

- **Overleaf (no install — the default, and the realistic route for a non-technical
  user):** upload the `.tex` (plus a photo if used) to a blank Overleaf project and press
  *Recompile* — the menu's default `pdfLaTeX` works as-is. Nothing to install.
- **Locally:** run `pdflatex resume.tex` **twice** (for cross-references), or
  `latexmk -pdf resume.tex` which handles that; then clean the aux files (`latexmk -c`, or
  remove `*.aux *.log *.out *.fls *.fdb_latexmk`).

### Minimal local install (scoped to this template)

The template is compiled with **`pdflatex`** — it uses pdfTeX-only primitives
(`\input{glyphtounicode}`, `\pdfgentounicode=1`) for an ATS-extractable text layer, so the
distribution must provide `pdflatex`. **Tectonic will not work**: it runs the XeTeX engine,
under which those primitives are undefined.

A local build needs only what *this* template uses — nothing more: the fonts
`CormorantGaramond` and `charter`, the `fontawesome5` icons, `textcomp`, and the output
language's `babel`/hyphenation. A bare `texlive-latex-base` is **not** enough (it lacks the
fonts and icons). Two routes:

- **TinyTeX (lightest — a minimal TeX Live with a real `pdflatex`, no sudo):** install it,
  then `tlmgr install fontawesome5 cormorantgaramond charter titlesec enumitem tabularx
  microtype hyperref geometry xcolor babel-<language> latexmk`.
- **TeX Live via a system package manager (Debian/Ubuntu example):**
  `texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
  texlive-fonts-extra texlive-lang-european`. The bulk is `texlive-fonts-extra`, where the
  template's font and icons live.

If a different template is chosen later with different dependencies, that template carries
its own install note — the requirement follows the template, not a fixed global install.

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
