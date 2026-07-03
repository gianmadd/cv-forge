# Roadmap

What's decided, what's left to build, and what's deliberately deferred.

## Done

- Project direction, scope, and full design — captured in [`decisions.md`](decisions.md).
- Career Profile specification — [`career-profile.md`](career-profile.md).
- Names fixed: repo `cv-forge`; skills `cv-profiler` and `cv-tailor`.
- Distribution model chosen: shared `skills` installer, no custom installer.
- Repo owner (`gianmadd`) and license (MIT, © Gian Marco Addati) decided.
- Repository documentation and scaffold (this `docs/` set, `README.md`, `CONTEXT.md`, `CHANGELOG.md`, `package.json`).

## To build

1. **`cv-profiler` (`skills/cv-profiler/SKILL.md`).** The interview: `Phase 0`
   calibration → profile note; the phased interview; the core/conditional structure with
   `PURPOSE` markers and their fixed authoring rule; the probe banks for early-career /
   career changers; gap noticing; the conditional-proposal rubric; incremental saving and
   the three-way dispatch; zero-fabrication and default rephrasing.
2. **`cv-tailor` (`skills/cv-tailor/SKILL.md`).** The generator: reads the Career Profile
   by the agreed section names, renders `Languages` as a conditional section, selects and
   structures content per job posting, matches keywords across languages, uses
   domain-adaptive verbs and localized section names, and compiles the output.
3. **Templates & examples.** Rewrite the CV / cover-letter templates from scratch (single
   neutral layout); add two example Career Profiles — one technical, one non-technical.
4. **Verification.** Run the lightweight eval (non-technical personas + acceptance
   checklist) and one end-to-end test per profile type.
5. **Cleanup before publishing.** Remove all baseline material and internal design notes;
   ensure deliverables read as original work.
6. **Publish.** Confirm the repo conforms to the `skills` convention, then publish and
   verify installation on Claude Code — followed by incremental checks on other agents.

## Deferred / open questions

- **Other agents beyond Claude Code** — handled by the shared installer; only
  *testing* remains, after the skills exist.

## Content to produce during implementation

Decided as rules; exact text written when the skills are authored: the `PURPOSE` texts
for every section, the probe-bank wording, the good/bad examples for the conditional
rubric, and the two example Career Profiles.
