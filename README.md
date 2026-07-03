# cv-forge

**Organize everything about your career once, then generate CVs that automated screening tools can actually read — tailored to each job.**

`cv-forge` is a pair of skills for AI coding agents that turn the scattered story of your working life into structured, job-ready CVs — without ever inventing anything about you. It's built and tested first on Claude Code; the shared installer also symlinks the skills into other AI coding agents.

---

## Why

Applying for a job today runs into two hard problems:

1. **Getting past the machines.** More and more applications are read first by automated, AI-powered screening tools. Writing a CV that these tools parse correctly — the right structure, the right keywords, no formatting that trips them up — is far from obvious, and getting it wrong can quietly sink a strong candidate.

2. **Organizing your own story.** Pulling together everything you've done — roles, results, skills, projects, education — into one coherent, well-structured picture is genuinely hard to do alone, especially across a long or varied career. Most people never have it all in one place.

`cv-forge` tackles both. First it helps you build a single, well-organized record of your career through a guided interview. Then it turns that record into CVs that are structured to be read cleanly by automated screening tools and tailored to each specific job — using only what you actually told it.

---

## Install

`cv-forge` is distributed as agent skills, installed with the shared `skills` CLI:

```bash
npx skills@latest add gianmadd/cv-forge
```

This installs two skills — `cv-profiler` and `cv-tailor` — into your agent. In Claude Code, invoke them as `/cv-profiler` and `/cv-tailor`.

---

## How it works

It runs in two stages, as a pipeline:

### 1. `cv-profiler` — build your Career Profile
A guided interview that captures everything about your career in one structured document: roles, responsibilities, achievements and metrics, skills, education, languages, and more. This document is your **single source of truth** — you build it once and keep it.

- Adapts its questions to your profession and background — it works for any field.
- Saves incrementally, so you can stop and pick up where you left off.
- Notices things worth clarifying (gaps, overlapping roles, transferable skills) and asks with tact — never assumes, never fills in blanks for you.

### 2. `cv-tailor` — generate a CV for a specific job
Give it your Career Profile plus a job posting, and it produces a CV (and, optionally, a cover letter) built for that role: structured so automated screening tools parse it correctly, and focused on the experience and keywords that fit the posting — drawing **only** from what's in your profile.

- Surfaces the right experience and keywords for the posting.
- Works in the language you choose.
- Never fabricates or estimates: if it isn't in your profile, it doesn't appear.

---

## Principles

- **Zero fabrication.** Every fact, metric, and claim comes from you — nothing is invented, embellished, or estimated.
- **Polished, never invented.** Your wording gets refined where it helps — tone, clarity, grammar, structure — but the facts underneath always stay yours.
- **Works for any field.** The interview and the output adapt to your profession rather than forcing a single template.
- **Multilingual.** The interview and the generated CV can be in the language you choose.

---

## Privacy

Your Career Profile contains personal information, and it stays **entirely on your machine**. Nothing is sent anywhere except explicit, transparent web searches — and those never include your personal data. You decide what to store, and you're responsible for deleting the file when you no longer need it.

---

## Documentation

- [`docs/architecture.md`](docs/architecture.md) — how the two skills fit together and how they're distributed.
- [`docs/career-profile.md`](docs/career-profile.md) — the structure of the Career Profile document.
- [`docs/principles.md`](docs/principles.md) — the rules both skills follow.
- [`docs/decisions.md`](docs/decisions.md) — the design decisions behind the project, with rationale.
- [`docs/roadmap.md`](docs/roadmap.md) — what's built, what's next, and open questions.
- [`CONTEXT.md`](CONTEXT.md) — shared vocabulary.

---

## Status

🚧 **In active development.** The skills are being written; this repository currently defines the project, its structure, and its direction.

---

## License

MIT — see [`LICENSE`](LICENSE).
