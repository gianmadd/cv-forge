# Import — turning an existing CV into a profile

Reach for this when the user has an **existing CV** to start from. Import produces the same
artifact as any other mode — a Career Profile — seeded from the document instead of from a
blank interview. It has two sub-flows, chosen by whether a profile already exists:

- **Import → New** — no profile file yet: extract, seed a new profile, review, interview.
- **Import → Enrich** — a profile already exists: extract, **reconcile into** it, review,
  interview. (Dispatched under Re-Run; see [`re-run.md`](re-run.md).)

Everything here obeys the skill's non-negotiable rules — above all **zero fabrication**:
you extract what is on the page, you do not clean up, infer, or complete it.

## Detecting a CV — offer, never grab

While reading the working directory for the dispatch, notice a file that looks like a CV
(a `.pdf/.docx/.odt/.rtf/.txt/.md` whose name hints cv / resume / curriculum). Make **one
soft offer** to import it — never extract on your own initiative. If several candidates
exist, list them and ask which; don't guess. If a profile already exists, frame the offer
as *"fold this CV into your profile?"*, not "start over".

**Anti-nag:** once a profile exists, record the outcome (imported, or declined) in the
provenance comment (below) so you don't re-offer the same file every session. Before a
profile exists there is nowhere to record a decline — re-offering next time is acceptable
(nothing is in the directory to remember against).

## Extraction

Prefer a **verbatim text-layer extraction** over reading the document by sight — a text
layer is faithful; a visual read re-types the content and can hallucinate.

- **Text/`.md`/`.tex`/`.html`** — read directly.
- **PDF / DOCX / ODT / RTF** — use `pdftotext -layout` (PDF) or `pandoc` (the rest). Mirror
  `cv-tailor`'s dependency flow: **detect → offer → propose the install for approval**,
  never install silently. If the tool is absent, fall back to the agent's native reading
  where it supports the format; DOCX with neither `pandoc` nor native support → tell the
  user to *install pandoc **or** re-export the CV to PDF/text*.
- **Scanned / image PDF (empty or garbage text layer)** — read it by sight **only with an
  explicit flag**: "this is a scan, I'm reading it visually, fidelity isn't guaranteed —
  please double-check". On an agent without vision, say plainly you can't read a scan there.
- **Keep all tooling local** — never send the document to a cloud OCR/parsing service
  (privacy).
- **Illegible spots** → `[ILLEGIBLE]`, never filled.
- **Not actually a CV** (a job posting, a cover letter) → flag it and ask, don't seed nonsense.
- **Durations and counts** follow the zero-fabrication rule in `SKILL.md`: extract what's
  stated; never decompose a stated total into per-role figures, never aggregate roles.

## Seeding the profile

- Write the profile immediately (incremental saving from the first write) — and **announce
  it**: "I'll save what I extract to `career-profile.md` so it isn't lost."
- Place extracted content in the matching core sections; **mark each seeded section
  `[TO CONFIRM]`** (present but unconfirmed). Sections the CV doesn't cover stay
  `[TO COMPLETE]`.
- Record the source in a **separate HTML comment** (not the calibration profile note),
  e.g. `<!-- SOURCE: seeded from cv.pdf on 2026-05-01 -->`.

## Import → Enrich — reconciling into an existing profile

- **Match and dedup** extracted entries against what's already there; don't append
  duplicates.
- **Propagate** per Re-Run rules (Work Experience is the primary source; aggregates
  re-derive).
- **Conflicts** (the CV says X, the profile says Y) are **surfaced to the user, never
  silently merged** — the user decides which is right.

## After seeding: review, then interview

1. Run the review muscle ([`review.md`](review.md)) over the seeded content — flags, not fixes.
2. Interview to resolve: **confirm the `[TO CONFIRM]` sections**, fill the `[TO COMPLETE]`
   gaps, address the review flags. Remove each marker as its section is confirmed / filled.

**Review-only is just the natural stop point.** Every import runs extract → seed → review,
then **offers** to continue into the interview. If the user only wanted a critique, deliver
the flags and stop — the seeded draft (with its markers) stays on disk, resumable later.

**Generate-now is another natural stop.** If the user wants a CV immediately — most often
when they handed over a CV *and* a job posting together — they need not finish the interview
first: point them to `cv-tailor` on the seeded draft. Its draft guard uses `[TO CONFIRM]`
content with an "unconfirmed — review it" caveat and skips still-empty sections, so a tailored
CV comes out now and the profile can be confirmed and filled later. Say this plainly so the
user knows the fast path exists and what its caveat means.
