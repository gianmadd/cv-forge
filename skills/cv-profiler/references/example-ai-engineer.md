<!-- PROFILE NOTE: Mid-to-senior AI/ML engineer (~8 yrs), she/her, LLM/NLP and MLOps focus.
     Confident technical track; emphasise model impact metrics, production reliability, and
     research-to-production ownership. Example/mock profile — not a real person. -->

<!-- PURPOSE: [CORE] How to reach the person and where they are based. Holds contact
     channels and current location only; not a summary of experience or availability. -->
## Contact Information

- **Name:** Elena Vásquez
- **Location:** Barcelona, Spain
- **Email:** elena.vasquez@example.com
- **LinkedIn:** linkedin.com/in/elenavasquez
- **GitHub:** github.com/elenavasquez

<!-- PURPOSE: [CORE] Target titles and the value proposition the person leads with — who
     they aim to be seen as. Not a recap of past roles (that lives in Work Experience). -->
## Professional Identity & Positioning

- **Target titles:** Senior Machine Learning Engineer · Staff ML Engineer · Applied Scientist (NLP)
- **Value proposition:** ML engineer who takes models from notebook to production and keeps
  them healthy there — pairing LLM/NLP depth with the MLOps discipline to serve them reliably
  and affordably at scale. Known for cutting inference cost and latency without sacrificing
  quality, and for making research reproducible.

<!-- PURPOSE: [CORE] A few genuinely different angles on the person's story, ready to reuse.
     Each is a distinct framing, not the same summary reworded; drawn from the detail below. -->
## Professional Summaries

- **LLM / NLP angle:** ML engineer with 8 years shipping NLP systems, most recently
  retrieval-augmented LLM features serving millions of queries a day. Raised answer accuracy
  40% on a customer-support assistant while halving its serving cost.
- **MLOps / production angle:** Applied ML engineer focused on the unglamorous half —
  reproducible training, evaluation harnesses, and monitoring — that keeps models trustworthy
  after launch. Built the platform that took a team's model releases from monthly to daily.
- **Research-to-production angle:** Comfortable reading a paper on Monday and having a
  defensible prototype by Friday; equally at home optimising C++ inference kernels and running
  the A/B test that proves the win.

<!-- PURPOSE: [CORE] Capability categories adapted to the person's field. Lists what they can
     do, grouped; does not re-explain where each skill was used (see Work Experience). -->
## Skills / Competencies

- **Languages:** Python, C++ (performance-critical inference), C#/.NET (earlier roles), SQL, Bash
- **ML / DL:** PyTorch, JAX, TensorFlow, Hugging Face Transformers, scikit-learn
- **LLM / NLP:** fine-tuning & LoRA, retrieval-augmented generation (RAG), embeddings, evaluation & guardrails
- **MLOps & data:** MLflow, Kubeflow, Airflow, Spark, Docker, Kubernetes, AWS SageMaker, PostgreSQL
- **Practices:** experiment design & A/B testing, model evaluation, R&D spikes, data pipelines, cross-functional delivery

<!-- PURPOSE: [CORE] The canonical, detailed record of roles held — the primary source every
     other section defers to. Full responsibilities and outcomes per role, ordered by end date. -->
## Work Experience

### Senior Machine Learning Engineer — Helix AI GmbH
*Berlin, Germany (remote) · April 2021 – Present*

- Own the retrieval-augmented LLM assistant behind customer support (Python, PyTorch, RAG over
  a 30M-document index); raised answer accuracy by 40% and cut hallucinated responses sharply
  through better retrieval and evaluation.
- Rewrote the hot path of the inference server in C++, cutting p95 latency in half and saving
  an estimated **$2M/year** in GPU serving cost.
- Built the team's evaluation and monitoring harness, moving model releases from monthly to daily.
- Led an R&D spike on on-device distillation that became a shipped low-latency tier.

> Agent Note: The $2M serving-cost saving was a joint effort with the platform team — Elena
> led the inference-kernel rewrite, but do not attribute the full saving to her alone. Phrase
> as "contributed to" the cost reduction, not "single-handedly saved $2M".

### Machine Learning Engineer — Barnes & Noble (Personalization)
*Remote (Spain) · June 2018 – March 2021*

- Built and shipped the recommender models (Python, TensorFlow, Spark) for the online store,
  lifting click-through on recommendations by 22%.
- Model won **#1** on the company's internal recommendation-quality leaderboard two quarters running.
- Introduced an offline evaluation pipeline that cut "bad launch" rollbacks by 60%.

### Software Engineer (ML Platform) — Corebyte Solutions
*Madrid, Spain · September 2016 – May 2018*

- Built data-ingestion and feature services (C#/.NET, SQL Server) feeding the company's first
  ML models.
- Automated the retraining pipeline, removing a full day of manual work each week.

<!-- PURPOSE: [CORE] Formal education, credentials, and training. Facts and dates only; the
     narrative of what was applied where belongs in Work Experience. -->
## Education & Training

- **M.Sc. Artificial Intelligence** — Universitat Politècnica de Catalunya (UPC), 2014 – 2016
- **B.Sc. Computer Engineering** — Universidad de Sevilla, 2010 – 2014
- **AWS Certified Machine Learning – Specialty**, 2022

<!-- PURPOSE: [CORE] A curated highlight reel of the person's most significant, quantified wins
     across their career. Not a repeat of every bullet above; the standout few. -->
## Key Achievements & Metrics

- Raised support-assistant answer accuracy **40%** while halving serving cost.
- Contributed to an estimated **$2M/year** GPU cost reduction via a C++ inference rewrite.
- Lifted recommendation click-through **22%**; **#1** internal recommendation-quality leaderboard.
- Cut bad-launch rollbacks **60%** with an offline evaluation pipeline.

<!-- PURPOSE: [CORE] Languages the person speaks and a self-assessed level for each. Descriptive
     self-assessment; no certification required. Appears on a CV only when beyond a native language. -->
## Languages

- Spanish — native
- English — C2 (fluent)
- German — B2 (upper-intermediate)
- Catalan — C1 (fluent)

<!-- PURPOSE: [CORE] Strategic positioning guidance for tailoring — which themes and angles to
     emphasise for which kinds of roles. Points to themes; holds no per-entry instructions. -->
## Notes for CV Customization

- For **applied-science / LLM** roles, lead with the RAG assistant, accuracy gains, and the
  research-to-production examples.
- For **platform / MLOps** roles, foreground the evaluation harness, monitoring, and the
  release-cadence improvement; the C++ inference work supports the "reliability + efficiency" story.
- Keep the early C#/.NET platform role brief unless the target role values data-platform breadth.

<!-- PURPOSE: [CONDITIONAL] Peer-reviewed or published output. Proposed for profiles with
     research/writing relevant to the target roles; skip when there is none. -->
## Publications & Writing

- Co-author, "Cheap Retrieval, Better Answers: Cost-Aware RAG for Support" — industry ML workshop, 2023.
- Writes an occasional blog on practical LLM evaluation.

<!-- PURPOSE: [CONDITIONAL] Public code contributions and their impact. Proposed when the person
     has meaningful open-source work relevant to the target roles. -->
## Open-Source Contributions

- Contributor to Hugging Face `evaluate` — added a metric for retrieval grounding.
- Maintains **rag-eval**, a small open-source toolkit for evaluating RAG pipelines.
