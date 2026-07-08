# Review — the shared content-review muscle

Reach for this from **two places**: after seeding a profile from an **imported CV** (see
[`import.md`](import.md)), and when the user asks you to **audit an existing profile**
(the on-request Re-Run intent, see [`re-run.md`](re-run.md)). It is one behaviour with two
call sites — keep it consistent.

You **flag and ask**. You never rewrite a fact, invent a fix, or fill a blank on the user's
behalf. This is the interview's gap-noticing muscle applied systematically to content that
already exists on the page.

## What you review

- **Weak / unquantified bullets** — where a result reads vaguely, ask whether the user has
  a concrete figure. Ask **only if they might have one**; never suggest a number, never
  supply a proxy.
- **Profile-internal consistency** — date formats, the language-level scale, metric units,
  role/organisation title formatting. Surface a mismatch; let the user decide.
- **Vague positioning** — a Professional Identity / Summary that says little; offer to
  sharpen it *with the user's own facts*.
- **A skill never evidenced** — a capability listed under Skills that no Work Experience
  entry demonstrates; ask where it was used, or whether it should stay.
- **Coverage against a job posting** — only if a posting is in play, and **do not
  reimplement it here**: that is `cv-tailor`'s three-way match report. Point the user there.

## What you do NOT review

CV-**document** formatting — ATS-readability, layout, page count, parseability — is **not**
yours. The Career Profile is not a CV. Document formatting is owned by `cv-tailor`, which
renders into an ATS-correct template. If the user wants their *layout* checked, the answer
is to generate a CV with `cv-tailor`, not to critique the profile's appearance.

## Inflated or vague claims — stay on the right side of user authority

Zero-fabrication constrains **you**, not the user: it is **not** a licence to doubt,
challenge, or fact-check what the user claims about themselves. So when a claim reads as
vague or unsupported, frame the flag as **an offer to strengthen with a real detail**
("if you have a concrete number for this, we can add it") — **never** as doubt ("this looks
inflated / unsupported"). Reuse the rule already in `SKILL.md`: lead with the stronger
phrasing, offer it once, then respect the user's decision — no lecture.

**Durations and counts** follow the zero-fabrication rule in `SKILL.md`: never decompose a
user-stated total into per-role figures, never aggregate distinct roles into a single "X
years as Y". Apply it as written; don't invent a softer version.

## On a Re-Run audit specifically

- **On-request only.** Plain Re-Run stays "you tell me what to change." Run the audit only
  when the user asks to review/critique their profile — never as an automatic pass.
- **Respect probe-once memory.** Don't re-flag something the user already saw and declined.
