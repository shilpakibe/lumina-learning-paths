# Problem Statement
### AI-Personalized Learning Paths — Lumina Learning Group

---

## Business Context

Lumina Learning Group (LLG) is a 1,000-person workforce learning and content-development services company, founded in 2011, generating $120M in annual revenue. LLG designs and delivers corporate learning content — instructional design, e-learning, simulations, and certifications — for clients across Pharma, Telecom, Banking/Finance, ERP/Software, and Call Center BPO verticals, operating out of Pune (India), Austin (USA), and Manchester (UK).

Client demand is shifting. Organizations no longer want generic, one-size-fits-all training catalogs — they want learning content that adapts to the individual employee's existing skill level, role, and pace. At the same time, LLG's own workforce is expected to build fluency in the AI/NLP/GenAI tools reshaping the instructional design profession itself. Leadership has identified this as a **competitiveness gap, not merely an efficiency opportunity**: competitors who can demonstrate AI-personalized learning at scale are winning client renewals that LLG is not.

## The Problem

LLG's current model matches employees to courses manually or through static, role-based catalogs. This approach has three structural weaknesses:

1. **No systematic link between what a role requires and what an employee has.** Required skills exist informally in job descriptions; actual employee proficiency exists informally in manager assessments. There is no structured way to compute the *gap* between the two.
2. **Course-to-skill matching is manual and inconsistent.** Course catalogs are tagged loosely, if at all, making it difficult to recommend the *right* course for a *specific* skill gap rather than a broad topic area.
3. **"Personalization" is currently a marketing claim, not a capability.** LLG cannot yet show a client — or its own workforce — a defensible, data-driven learning path that is genuinely different from one employee to the next based on their actual competency profile.

## What This Project Builds

A simulated, end-to-end system that solves this for two representative job families — **Instructional Design** and **Content/Solution Architecture** — by:

1. Defining measurable **competencies and KSBs** (Knowledge, Skill, Behavior) tied to LLG's strategic goals
2. Establishing **required proficiency levels per role**, deliberately asymmetric to reflect real differences between roles (not uniform, unrealistic ratings)
3. Modeling **employee competency gaps** against those requirements
4. Building an **NLP-based matching engine** that maps a course catalog to specific KSBs using keyword/similarity techniques (TF-IDF, content-based filtering), validated against both synthetic and real-world text, and benchmarked against a supervised course-to-competency classifier
5. Generating **personalized learning paths** — ranked, sequenced course recommendations — that visibly differ between roles because the underlying gaps genuinely differ
6. **Flagging near-duplicate courses** in the catalog, the first check a real L&D team makes when reviewing an existing content library

This system is grounded in established L&D theory, not just applied ML: the assessment includes **COM-B** (Capability, Opportunity, Motivation — Behavior) questions, a behavioral-science model used to diagnose *why* a skill gap exists, not just *that* it exists. A **Logic Model** (Inputs→Activities→Outputs→Outcomes→Impact, see `logic_model.md`) documents the project's honest scope boundary: it builds through the Output stage, and does not simulate Outcomes or Impact, since that would require fabricating longitudinal behavior-change data with no real grounding.

## Why This Matters (and Why Now)

This system directly serves two of LLG's stated strategic goals:
- **Launch GenAI-personalized learning paths as a client-facing offering** (client competitiveness)
- **Upskill the internal Content Dev workforce in DS/ML/NLP/GenAI** (internal capability)

It is deliberately built so that later phases can extend it without a schema rebuild: the same data model, with swappable matching logic.

## Success Criteria for This Simulation

- Two employees in different roles, with different competency gaps, receive **visibly different, defensible learning paths** — not the same course list reordered
- The KSB-to-course matching is **explainable**: for any recommendation, it's possible to show *why* that course was matched to that gap
- The system is **reproducible**: identical inputs (fixed random seed) produce identical synthetic data and identical demo output
- The data model requires **no restructuring** to extend in later phases

## Roadmap

- Phase 2: GenAI upgrade, coming soon
- Phase 3: coming soon

---

*This is a self-directed portfolio simulation using a fictional company and synthetic data, built to demonstrate applied DS/ML/NLP/GenAI skills within an authentic L&D/instructional-design domain context.*
