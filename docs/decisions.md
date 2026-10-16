# Decision Log — Lumina Learning Group: AI-Personalized Learning Paths

ADR-style (Architecture Decision Record) log. Each entry is self-contained enough to explain the full reasoning behind a decision on its own, without needing outside context. Config parameters live in `config.json`; this file is the *why*.

## Index

| ADR | Title | Status |
|---|---|---|
| [ADR-0001](#adr-0001-two-artifact-documentation-strategy) | Two-artifact documentation strategy (config.json + decisions.md) | Accepted |
| [ADR-0002](#adr-0002-synthetic-course-catalog-over-web-scraping) | Synthetic course catalog over web scraping | Accepted |
| [ADR-0003](#adr-0003-real-excerpt-credibility-sample-for-nlp-proof-of-concept) | Real-excerpt credibility sample for NLP proof-of-concept | Accepted |
| [ADR-0004](#adr-0004-two-job-families-selected-third-parked-for-later) | Two job families selected, third parked for later | Accepted |
| [ADR-0005](#adr-0005-research-incubator-distributed-across-all-three-offices) | Research Incubator distributed across all three offices | Accepted |
| [ADR-0006](#adr-0006-japan-as-client-market-not-a-fourth-office) | Japan as client market, not a fourth office | Accepted |
| [ADR-0007](#adr-0007-two-strategic-goals-selected-from-five-with-four-kpis) | Two strategic goals selected from five, with four KPIs | Accepted |
| [ADR-0008](#adr-0008-bimodal-asymmetric-required-proficiency-levels-by-role) | Bimodal/asymmetric required proficiency levels by role | Accepted |
| [ADR-0009](#adr-0009-master-employee-table-generated-independent-of-role-focus) | Master employee table generated independent of role focus | Superseded in part by ADR-0032 |
| [ADR-0010](#adr-0010-schema-designed-with-explicit-phase-1--phase-2-swap-points) | Schema designed with explicit Phase 1 → Phase 2 swap points | Accepted |
| [ADR-0011](#adr-0011-google-colab--drive-as-build-environment-github-as-migration-target) | Google Colab + Drive as build environment, GitHub as migration target | Accepted |
| [ADR-0012](#adr-0012-notebook-naming-convention) | Notebook naming convention | Accepted |
| [ADR-0013](#adr-0013-fixed-random-seed-for-reproducibility) | Fixed random seed for reproducibility | Accepted |
| [ADR-0014](#adr-0014-finalized-github-ready-folder-structure-supersedes-earlier-draft) | Finalized GitHub-ready folder structure (supersedes earlier draft) | Accepted |
| [ADR-0015](#adr-0015-master_promptmd-lives-in-claude-knowledge-base-only-not-drivegithub) | `master_prompt.md` lives in Claude knowledge base only, not Drive/GitHub | Accepted |
| [ADR-0016](#adr-0016-gdpr-by-design-privacy-approach) | GDPR-by-design privacy approach (build vs. document-only) | Accepted |
| [ADR-0017](#adr-0017-supervised--unsupervised-ml-technique-selection-across-phases) | Supervised + unsupervised ML technique selection across phases | Accepted |
| [ADR-0018](#adr-0018-simulated-competency-assessment-design) | Simulated competency assessment design (long format, COM-B + proficiency combined, two cycles) | Accepted |
| [ADR-0019](#adr-0019-three-phase-roadmap-with-reuse-chain-and-upfront-data-generation) | Three-phase roadmap with reuse-chain principle and upfront data generation | Accepted |
| [ADR-0020](#adr-0020-scope-boundary-for-later-phases) | Scope boundary for later phases | Accepted |
| [ADR-0021](#adr-0021-logic-model-adopted-as-evaluation-framework) | Logic Model adopted as evaluation framework | Accepted |
| [ADR-0022](#adr-0022-two-stage-architecture-diagram) | Two-stage architecture diagram (draft, then polished) | Accepted |
| [ADR-0023](#adr-0023-b13-sketch-corrections-tf-idf-as-sole-production-matching-pathway) | B13 sketch corrections — TF-IDF as sole production matching pathway | Accepted |
| [ADR-0024](#adr-0024-plain-language-annotation-layer-for-the-architecture-diagram) | Plain-language annotation layer for the architecture diagram | Accepted |
| [ADR-0025](#adr-0025-schema-convention-set) | Schema convention set (PK/FK style, timestamps, phase-tagging, reserved columns, documented enums) | Accepted |
| [ADR-0026](#adr-0026-two-convention-exceptions-proficiency_scale-and-role_competency_requirements) | Two convention exceptions — `proficiency_scale` integer PK, `role_competency_requirements` junction table | Accepted |
| [ADR-0027](#adr-0027-full-18-table-field-level-schema-closed) | Full 18-table field-level schema closed | Accepted |
| [ADR-0028](#adr-0028-ksb-list-finalized-at-30) | KSB list finalized at 30 (not ~29) | Accepted |
| [ADR-0029](#adr-0029-vertical-field-added-to-employees) | `vertical` field added to `employees` | Accepted |
| [ADR-0030](#adr-0030-multi-category-learning-paths-documented-as-roadmap-not-built) | Multi-category learning paths documented as roadmap, not built | Accepted |
| [ADR-0031](#adr-0031-course_source-field-added-to-courses-simulated-ingestion-layer) | `course_source` field added to `courses` — simulated ingestion layer | Accepted |
| [ADR-0032](#adr-0032-employees-table-scoped-to-430-real-org-chart-grounded-headcount-not-1000) | `employees` table scoped to 430, real-org-chart-grounded headcount, not ~1000 | Accepted (supersedes part of ADR-0009) |
| [ADR-0033](#adr-0033-nb01-drive-mount-cell-made-idempotent) | NB01 Drive-mount cell made idempotent — fixes live-mount data loss | Accepted |
| [ADR-0034](#adr-0034-course-catalog-rebuilt-as-a-procedural-generator-scaled-to-150) | Course catalog rebuilt as a procedural generator, scaled to ~150 | Accepted (supersedes part of ADR-0002, ADR-0031) |
| [ADR-0035](#adr-0035-embeddings-comparison-and-duplicate-detection-pulled-forward-into-nb02) | Embeddings comparison and duplicate-detection pulled forward into NB02 | Accepted (amends ADR-0010) |
| [ADR-0036](#adr-0036-human-in-the-loop-curation-layer-and-kpi-impact-rollup-added) | Human-in-the-loop curation layer and KPI Impact Rollup added | Accepted |
| [ADR-0037](#adr-0037-kpi_competency_mapping-added-with-kpi_type-typing-and-k22-reworded-to-annual-cadence) | `kpi_competency_mapping` added, KPIs typed by lineage, K2.2 reworded to annual cadence | Accepted |
| [ADR-0038](#adr-0038-course-target-labels-persisted-in-courses-at-generation-time) | Course target labels (`target_ksb_id`, `target_competency_id`) persisted at generation time | Accepted |
| [ADR-0039](#adr-0039-supervised-classifier-benchmark-is-non-discriminating-on-synthetic-data) | Supervised classifier benchmark is non-discriminating on synthetic data | Accepted |
| [ADR-0040](#adr-0040-lda-topic-count-chosen-by-coherence-after-data-driven-filler-word-removal) | LDA topic count chosen by coherence, after data-driven filler-word removal | Accepted |
| [ADR-0041](#adr-0041-duplicate-detection-by-embedding-similarity-threshold-and-connected-components) | Duplicate detection by similarity threshold and connected components | Accepted |
| [ADR-0042](#adr-0042-rule-based-curation-stand-in-with-an-open-resolution-question) | Rule-based curation stand-in, with an open resolution question | Accepted (open item resolved by ADR-0043) |
| [ADR-0043](#adr-0043-ld-curation-resolved-through-a-blind-side-by-side-review-sheet) | L&D curation resolved through a blind side-by-side review sheet | Accepted (resolves ADR-0042 open item) |
| [ADR-0044](#adr-0044-content-gaps-shown-as-coming-soon-path-steps) | Content gaps shown as "coming soon" path steps | Accepted |
| [ADR-0045](#adr-0045-path-sequencing-rules) | Path sequencing rules (gap size, K-S-B order, shared course, duplicate check) | Accepted |
| [ADR-0046](#adr-0046-kpi-impact-rollup-computed-in-nb03-with-two-projections) | KPI Impact Rollup computed in NB03 with two projections | Accepted |
| [ADR-0047](#adr-0047-source-of-truth-fixes-in-nb01-and-re-run-guards) | Source-of-truth fixes in NB01 and re-run guards | Accepted (amends ADR-0032, ADR-0037) |
| [ADR-0048](#adr-0048-privacy-demo-and-gdpr-aligned-wording) | Privacy demo and "GDPR-aligned" wording | Accepted (extends ADR-0016) |
| [ADR-0049](#adr-0049-cc-by-nc-40-licence-replaces-planned-mit) | CC BY-NC 4.0 licence replaces planned MIT | Accepted (supersedes MIT choice in ADR-0014) |
| [ADR-0050](#adr-0050-path-phasing-over-annual-cycles-with-two-caps) | Path phasing over annual cycles, with two caps | Accepted |
| [ADR-0051](#adr-0051-nb04-figure-conventions) | NB04 figure conventions | Accepted |
| [ADR-0052](#adr-0052-demo-employee-selection-and-the-privacy-gate-on-charts) | Demo employee selection and the privacy gate on charts | Accepted (extends ADR-0016, ADR-0048) |
| [ADR-0053](#adr-0053-version-2-backlog-kept-as-a-private-file) | Version 2 backlog kept as a private file | Accepted |
| [ADR-0054](#adr-0054-two-layer-notes-plain-line-then-under-the-hood) | Two-layer notes ("plain line, then Under the hood") | Accepted |
| [ADR-0055](#adr-0055-minimum-group-size-for-aggregate-views) | Minimum group size for aggregate views (small-group suppression) | Accepted (extends ADR-0016, ADR-0048) |

---

## ADR-0001: Two-artifact documentation strategy

**Context:** This project spans multiple threads (ideation, schema, 4 notebooks, README, GitHub setup), following the same pattern used successfully in the NLPCapstone01 project. Without a continuity mechanism, each new thread would need lengthy re-explanation.

**Options considered:**
1. Rely on conversation history / project memory alone.
2. Maintain a single combined doc mixing parameters and rationale.
3. Maintain two separate artifacts: `config.json` (machine-readable parameters/state) and `decisions.md` (human-readable ADR rationale log), cross-referenced by ID.

**Decision:** Option 3, directly mirroring the NLPCapstone01 pattern.

**Rationale (plain language):**
- Memory scope is per-project and not guaranteed to carry forward every micro-decision; explicit files are portable and auditable.
- Separating "what" (config) from "why" (decisions) keeps each file focused and scannable.
- This exact pattern was already proven to work well in the learner's prior capstone project.

**Status:** Accepted

---

## ADR-0002: Synthetic course catalog over web scraping

**Context:** The project needs a course catalog for the KSB↔course NLP matching engine. Initial idea was to scrape free online course listings.

**Options considered:**
1. Scrape real course platforms directly for content.
2. Build a fully synthetic/fake course repository (fake names, fake metadata, fake descriptions).
3. Hybrid — synthetic catalog as backbone, small real-excerpt sample for credibility.

**Decision:** Option 3 — synthetic catalog (30-50 courses) as the primary dataset; see ADR-0003 for the real-excerpt companion piece.

**Rationale (plain language):**
- Scraping carries legal/ToS risk and produces an unstable dataset (links break, content changes) — bad for a reproducible portfolio demo.
- A fully synthetic catalog gives full control over keyword density, coverage of KSBs, and long-term stability.
- Losing all connection to real-world text would undercut the "this works on authentic language" credibility argument — hence the hybrid.

**Status:** Accepted

---

## ADR-0003: Real-excerpt credibility sample for NLP proof-of-concept

**Context:** Following ADR-0002, a small set of real-world text samples was wanted to demonstrate the keyword/NLP matching pipeline works on authentic (non-authored-to-fit) text.

**Options considered:**
1. Skip real text entirely, rely only on synthetic catalog.
2. Use real course descriptions/blurbs (Coursera, edX) and/or arXiv abstracts as a small (3-5 item), clearly-labeled sample.

**Decision:** Option 2 — 3-5 real excerpts, kept as a separate NLP proof-of-concept sample, explicitly labeled "sample real-world text — demonstration of matching accuracy only, no commercial use."

**Rationale (plain language):**
- Small excerpts with attribution, no redistribution of full text, and clear non-commercial demo framing are the practical factors that matter here, more than disclaimer wording alone.
- Public arXiv papers are a natural source for NLP work, strengthening the NLP storyline.
- Keeping this separate from the main synthetic catalog avoids entangling licensing considerations into the core, always-reproducible dataset.

**Status:** Accepted

---

## ADR-0004: Two job families selected, third parked for later

**Context:** The org chart supports many possible job families (Content Dev, PMO, Business Dev, Coding, Testing, etc.). The project needed to pick a scoped subset for Phase 1.

**Options considered:**
1. Model all job families in the org chart.
2. Model 2 families directly relevant to the AI/DS/NLP/GenAI showcase (Instructional Design; Content/Solution Architecture).
3. Model 3 families, adding Project Management or Business Development for broader coverage.

**Decision:** Option 2 for Phase 1 — Instructional Design and Content/Solution Architecture. Project Management (framed as "AI-augmented delivery") explicitly parked as a possible future extension.

**Rationale (plain language):**
- Content Dev roles naturally carry DS/ML/NLP/GenAI-relevant competencies; PM/Biz Dev roles would either need artificially force-fit AI competencies or dilute the AI showcase with generic PM competencies.
- Two families with genuinely different skill profiles (content-authoring-heavy vs. systems-architecture-heavy) create visible contrast in demo output without excessive build scope.
- Naming a 3rd family as a possible extension costs one sentence and shows the system was designed to generalize, without the build cost.

**Status:** Accepted

---

## ADR-0005: Research Incubator distributed across all three offices

**Context:** The uploaded org chart shows a single centralized Research Director + Researchers unit (Innovation Incubator), reporting to CEO. The question was whether to keep this centralized or distribute researchers across the 3 offices.

**Options considered:**
1. Keep Innovation Incubator centralized in one office (e.g., Pune only).
2. Distribute researchers across Pune, Austin, and Manchester.

**Decision:** Option 2 — distributed across all 3 offices.

**Rationale (plain language):**
- A distributed research function gives a "360-degree perspective" narrative: Pune contributes core R&D/engineering talent, Austin contributes North America market/client research, Manchester contributes EU/UK compliance and regulatory learning trends.
- More realistic for a global services firm, and reflects cross-timezone collaboration, a real feature of global L&D teams.

**Status:** Accepted

---

## ADR-0006: Japan as client market, not a fourth office

**Context:** Discussion arose about whether to add APAC representation (e.g., Australia or Japan) as a 4th physical office, in addition to Pune/Austin/Manchester.

**Options considered:**
1. Add a 4th physical office (AU or Japan).
2. Add Japan as a client market only, served out of the existing Pune office, with no new physical location.

**Decision:** Option 2.

**Rationale (plain language):**
- A 4th office adds org complexity (more headcount rows, more KPI/scenario text) without adding much value to the core AI/NLP showcase, which is the actual point of the project.
- More offices means more synthetic data to keep consistent — extra maintenance for a portfolio piece with low payoff.
- Naming Japan as a served client market (e.g., an AI-personalized learning pilot for a Japanese manufacturing client) gets the APAC flavor into the scenario narrative at effectively zero build cost.

**Status:** Accepted

---

## ADR-0007: Two strategic goals selected from five, with four KPIs

**Context:** Five strategic goals were drafted for LLG. The question was whether to build KPIs/competencies/KSBs for all 5, or narrow scope.

**Options considered:**
1. Build out all 5 goals with full KPI trees.
2. Show all 5 goals for narrative completeness, but derive KPIs/competencies/KSBs for only 2 selected goals.

**Decision:** Option 2 — Goal 1 (Launch GenAI-personalized learning paths, client-facing) and Goal 2 (Upskill Content Dev workforce in DS/ML/NLP/GenAI) selected as Phase 1 active goals, 2 KPIs each (4 total). Remaining 3 goals documented as parked/future scope.

**Rationale (plain language):**
- Showing all 5 goals preserves a complete, realistic strategic narrative (demonstrates full L&D scope understanding).
- Narrowing execution to 2 goals keeps the downstream competency/KSB/data build manageable within the project's disciplined MVP scope.
- 4 KPIs (2 per goal) is enough to drive meaningful downstream competency requirements without over-building a KPI tree that the demo doesn't actually use.

**Status:** Accepted

---

## ADR-0008: Bimodal/asymmetric required proficiency levels by role

**Context:** Needed to decide whether the 10 competencies should have similar required-proficiency levels across both roles, or be deliberately weighted/differentiated.

**Options considered:**
1. Apply similar/uniform required levels across both roles for all competencies.
2. Weight competencies differently per role to mirror real business scenarios, producing visibly divergent profiles.

**Decision:** Option 2 — each role has 2-3 competencies at Level 4 (core expertise) and 1-2 at Level 1-2 (genuinely not their job), with overlap competencies (data, taxonomy, QA) kept close but not identical.

**Rationale (plain language):**
- Real organizations don't rate every role "pretty good at everything" — uniform ratings are a common tell of unrealistic synthetic data.
- Asymmetric extremes (e.g., ID Lead: Level 4 on Instructional Design Fundamentals / Level 1 on Solution Architecture, and the reverse for Solution Architect) produce genuinely divergent KSB gaps and thus visibly different recommended learning paths — the core visual proof-of-personalization for the demo.
- This decision directly determines whether the eventual matching/path-generation output looks meaningfully personalized or just like the same list reordered.

**Status:** Accepted

---

## ADR-0009: Master employee table generated independent of role focus

**Context:** Question arose on whether to generate employee data only for the 2 spotlighted roles, or build a master table covering the full ~1000-person org.

**Options considered:**
1. Generate employee data only for the 2 Phase 1 roles (150-300 employees).
2. Generate a master table of all ~1000 employees org-wide, usable for any future role/family extension, in addition to the role-specific competency/gap data needed for Phase 1.

**Decision:** Option 2 — a master `employees` table covering the full ~1000-person org is generated as a reusable base input; role-specific `employee_competency_levels` and derived gap/recommendation data are still scoped to the 2 Phase 1 roles for the demo itself.

**Rationale (plain language):**
- Keeps the door open for future role/job-family extensions (per ADR-0004's parked 3rd family) without needing to regenerate base employee data later.
- Base employee attributes (role, office, tenure, certifications) are cheap to generate at full org scale; the expensive/detailed synthetic data (competency levels, gaps, recommendations) stays scoped to what the Phase 1 demo actually needs.

**Status:** Superseded in part by ADR-0032 — Thread 3 (NB01 build) found the user's real org chart (`20260907OPModel.xlsx`) grounds an exact headcount of 430 for the 4 roles under the 2 in-scope job families (JF01/JF02), not an arbitrary ~1000. The broader org-wide roster (PM, Business Development, Coding, Graphics, etc. — the other ~570 headcount) is deliberately spun out as a separate, standalone reusable asset outside this project's schema, rather than folded into `employees.csv`. The core principle here (generate a master table independent of role-specific demo needs) still holds — only the exact count and scope of "master" changed.

---

## ADR-0010: Schema designed with explicit Phase 1 → Phase 2 swap points

**Context:** The learner wanted the Phase 1 (TF-IDF/rule-based) system to be extendable later without a schema rebuild.

**Options considered:**
1. Build the Phase 1 schema without regard to later extension; redesign when needed.
2. Design the schema now with explicit "swappable" fields that record which method produced a row, using the same table structure throughout.

**Decision:** Option 2.

**Rationale (plain language):**
- Costs almost nothing at design time, saves a full rebuild later.
- `course_ksb_mapping.match_method` and `match_explanation`, plus `learning_path_recommendations.recommendation_method` and `rationale`, record which method produced each row (`tfidf`, `embedding`, `rule_based` in Phase 1). A new method is added as a new value, not a new column.
- `courses.description_raw` stores full text (not just extracted keywords), so no re-collection is ever needed.
- Matching/recommendation logic is treated as a swappable function behind a stable interface (e.g., `generate_matches(ksbs, courses, method="tfidf")`), a deliberate software-engineering choice.

**Status:** Accepted

---

## ADR-0011: Google Colab + Drive as build environment, GitHub as migration target

**Context:** Learner has no prior Colab or GitHub experience (background is Purdue's LMS-provided computing environment). Needed to decide the build/storage environment before writing any notebook code.

**Options considered:**
1. Build directly against a local environment.
2. Build in Google Colab with session-local (ephemeral) storage, re-running/re-saving each session.
3. Build in Google Colab with Google Drive mounted for persistent storage, migrating the finished project to GitHub afterward.

**Decision:** Option 3.

**Rationale (plain language):**
- Colab requires no local installation, matching the learner's prior experience with a hosted LMS coding environment.
- Drive-mounted storage means synthetic data persists across sessions without needing to regenerate it each time.
- Confirmed working root path: `/content/drive/MyDrive/Portfolio/LuminaLearningGroup`.
- GitHub is the eventual home for the portfolio piece, but is deliberately treated as a separate, later migration step (its own thread) rather than something notebooks need to account for from day one.

**Status:** Accepted

---

## ADR-0012: Notebook naming convention

**Context:** Needed a consistent naming pattern for notebook files across the project.

**Options considered:**
1. Descriptive names without a fixed prefix (e.g., "Data Generation.ipynb").
2. Numbered prefix convention `NB##_topic_subtopic`, matching the learner's established personal convention (used consistently since their first job) and consistent with the NLPCapstone01 project's numbered-notebook pattern.

**Decision:** Option 2 — `NB01_synthetic_data_generation`, `NB02_nlp_ksb_course_matching`, `NB03_path_generation_recommendation`, `NB04_visualization_demo`.

**Rationale (plain language):**
- Matches a naming convention the learner has used consistently across their career — no new habit to adopt.
- Numbered prefixes make notebook execution order unambiguous, both in Colab and later on GitHub.

**Status:** Accepted (superseded in part by ADR-0014, which finalizes the folder locations for the same files).

---

## ADR-0013: Fixed random seed for reproducibility

**Context:** All synthetic data generation involves randomness (employee attributes, competency levels, tenure, etc.). Needed a reproducibility standard.

**Options considered:**
1. Leave randomness unseeded (data differs on every regeneration).
2. Fix a random seed across all generation steps, documented once.

**Decision:** Option 2 — `random_state = 42`, applied consistently across all synthetic data generation.

**Rationale (plain language):**
- Matches the convention already used in the learner's NLPCapstone01 project (`random_state = 42` for LDA/model training there), maintaining consistency across their portfolio.
- Ensures anyone cloning the eventual GitHub repo and re-running the notebooks gets identical synthetic data and identical demo output — important for a project a reviewer might actually run.

**Status:** Accepted

---

## ADR-0014: Finalized GitHub-ready folder structure (supersedes earlier draft)

**Context:** An earlier draft (documented informally earlier in Thread 1) had `config.json`, `decisions.md`, `master_prompt.md`, and `SETUP_INSTRUCTIONS.md` all sitting loose at the project root, modeled on an initial (and, on inspection, incorrect) reading of the NLPCapstone01 Purdue LMS folder layout. Reviewing an actual screenshot of that LMS layout showed `config.json` lives inside a `Config/` folder there, not loose at root. Separately, the question arose of what folder structure would look best specifically for a public GitHub audience, not just what matched the prior project.

**Options considered:**
1. Keep the original loose-files-at-root draft.
2. Exactly mirror the Purdue LMS folder names (`Config`, `Datasets`, `Outputs`, `coursework`).
3. Adopt an industry-standard, GitHub-idiomatic layout (close to the common "cookiecutter data science" template), keeping the user's preferred `notebooks/` subfolder (an improvement over the LMS layout, where notebooks sat loose at root) and adding standard GitHub-facing files (`README.md`, `LICENSE`, `requirements.txt`, `.gitignore`) not present in the LMS version at all.

**Decision:** Option 3.

**Rationale (plain language):**
- The Purdue LMS layout was designed for a coursework submission environment, not for a public GitHub audience — the two have different "first impression" requirements.
- A minimal, scannable root (README + a small number of top-level folders) reads as more professional to a reviewer than a root cluttered with multiple loose markdown files, even though the LMS version had loose docs at root successfully.
- `config.json` → `config/` folder, `decisions.md` and `SETUP_INSTRUCTIONS.md` → `docs/` folder (not loose at root) — this keeps root minimal while still making these docs one click away, not buried.
- `data/derived` renamed to `data/processed` to match the far more common raw → processed naming convention used across public data science repos.
- Added `reports/figures/` for exported chart images the README will embed (GitHub renders markdown but not live Python plots, so saved images are required).
- Added `LICENSE` (MIT, to be filled in during the README/Packaging thread) and `requirements.txt` — commonly checked-for signals of a professionally maintained repo.
- Added `.gitignore` to keep Colab checkpoint files and OS artifacts out of version control.
- The `notebooks/` subfolder (the user's own preference, not present in the LMS layout) is retained and considered an improvement over the LMS pattern.
- The Google Drive working folder and the eventual GitHub repo root are designed to map 1:1, so no restructuring will be needed at GitHub migration time.

**Migration executed via:** `01_migrate_folder_structure.py` (idempotent script — renames `data/derived` to `data/processed`, creates `config/` and `docs/` (with `docs/ADRs/` nested inside), moves `config.json` and the three `.md` files into their new locations if present, creates `reports/figures/`, and adds placeholder `LICENSE`/`requirements.txt`/`.gitignore`).

**Status:** Accepted

---

## ADR-0015: `master_prompt.md` lives in Claude knowledge base only, not Drive/GitHub

**Context:** `master_prompt.md` was originally drafted alongside `config.json`/`decisions.md`/`SETUP_INSTRUCTIONS.md` as a file to be uploaded into the Drive project folder (and by extension, eventually GitHub). On reflection, its actual purpose is different from the other three files.

**Options considered:**
1. Treat `master_prompt.md` the same as `decisions.md`/`SETUP_INSTRUCTIONS.md` — store it in `docs/` in Drive, ship it to GitHub.
2. Treat it as a private working document between the user and Claude — pasted at the start of every new thread for continuity, stored only in this project's Claude knowledge base, never uploaded to Drive or committed to the eventual GitHub repo.

**Decision:** Option 2.

**Rationale (plain language):**
- `config.json`, `decisions.md`, and `SETUP_INSTRUCTIONS.md` are genuine project artifacts — they describe the project itself and are meaningful to any reader (a GitHub visitor, a future collaborator).
- `master_prompt.md` is closer to a prompt-engineering/onboarding script for the AI-assisted workflow used to build the project — useful for continuity between Claude threads, but not something a GitHub reviewer needs to see, and not part of the project's technical deliverable.
- Keeping it out of GitHub keeps the repo focused on the project itself. The collaboration is disclosed in the README.

**Status:** Accepted


---

## ADR-0016: GDPR-by-design privacy approach

**Context:** The original schema had no explicit privacy design — employee PII (names, emails) sat alongside operational/competency data in one flat table, and no access pattern distinguished individual-level views from organizational reporting. While this project uses only synthetic data with no real GDPR obligation, the learner wanted the design to reflect real-world privacy-by-design principles as a portfolio credibility signal.

**Options considered (five candidate controls, evaluated individually):**
1. Split `employees_pii` (name, email) from `employees` (operational fields), keyed by `employee_id` only.
2. Per-employee-only access pattern in the demo notebooks (simulated "login as employee X," no full-table dumps of individual gaps/recommendations).
3. Aggregate-only organizational dashboards (counts/percentages only, never individual rows).
4. Formal data-minimization audit of the `employees` table fields.
5. Full privacy framework (encryption at rest, consent management, right-to-erasure, retention policy).

**Decision:** Build options 1-3 into the schema/pipeline. Document options 4-5 as roadmap markdown, each naming the real-world technique/tool that would implement it at production scale (e.g., differential privacy libraries, k-anonymity techniques, a formal DPA) — not built, but named specifically enough to show awareness of the production-grade solution.

**Rationale (plain language):**
- Options 1-3 are structural — they change how the data is organized and accessed, cost little to build, and directly demonstrate privacy-by-design thinking in the actual pipeline output, which is what a reviewer would actually see.
- Options 4-5 require either extensive schema audit work or production-grade infrastructure with no natural payoff in a synthetic, single-user portfolio context — better to name them explicitly (showing awareness) than build them as theater.
- This mirrors the project's existing discipline (see ADR-0002, ADR-0003) of not fabricating complexity that has no real grounding, while still surfacing the more advanced approach as a documented "next step."

**Status:** Accepted

---

## ADR-0017: Supervised + unsupervised ML technique selection across phases

**Context:** The learner's Purdue certification spans DS, ML, DL, NLP, and GenAI, and wanted the project to showcase both supervised and unsupervised techniques, not just NLP matching, without diluting the project's core narrative or requiring synthetic data of a scale the project doesn't have.

**Options considered:**
- *Supervised*: (a) course→competency classifier vs. TF-IDF comparison, (b) KSB gap-closure prediction, (c) learner satisfaction prediction.
- *Unsupervised*: (a) employee competency clustering, (b) LDA topic modeling on the course catalog, (c) PCA/t-SNE visualization.

**Decision:** Supervised: course→competency classifier, directly benchmarked against the TF-IDF matching approach (Phase 1). Unsupervised: LDA topic modeling as a secondary lens alongside TF-IDF matching (Phase 1). KSB gap-closure prediction was not built in Phase 1. Learner satisfaction prediction and employee clustering were not adopted — no natural data foundation and limited insight at this data scale, respectively.

**Rationale (plain language):**
- The classifier-vs-TF-IDF comparison requires no new data generation (courses need competency labels either way) and directly reinforces the project's core NLP story with a genuine supervised/unsupervised trade-off narrative.
- LDA parallels the learner's NLPCapstone01 project, creating a portfolio continuity/growth story.
- Employee clustering was set aside because, with only ~150-300 employees across 2 intentionally bimodal roles (ADR-0008), clustering would likely just rediscover "there are two roles" — a low-insight result for the effort.
- Learner satisfaction prediction was set aside because it has no natural connection to the KSB/competency/course machinery already being built — it would pull scope into disconnected HR-analytics territory.

**Status:** Accepted

---

## ADR-0018: Simulated competency assessment design

**Context:** Competency levels needed a real (synthetic) data source, so a simulated questionnaire was designed. Alongside proficiency, the learner wanted to include COM-B (Capability, Opportunity, Motivation), a behavioural-science model that explains why a skill is or is not applied, not only whether it exists. The design question: collect COM-B in a separate assessment event, or together with proficiency in one event, as real annual assessments usually do.

**Options considered:**
1. Proficiency-only questionnaire, with COM-B collected in a separate event.
2. One combined annual questionnaire (proficiency and COM-B questions together).
3. Wide format (one row per employee, one column per question) vs. long format (one row per employee-question pair).
4. One assessment cycle vs. two cycles (this year and last year).

**Decision:** Option 2: one combined annual assessment (`assessment_questions` lookup table + `assessment_responses` fact table). Phase 1 paths read only the proficiency rows; COM-B rows are collected and held for later use. Long format, tagged by `question_type` (`proficiency`, `comb_capability`, `comb_opportunity`, `comb_motivation`). Two assessment cycles, generated together in NB01.

**Rationale (plain language):**
- Real annual assessments collect everything in one event. A separate COM-B event would be less realistic, not more.
- Each COM-B question is linked to a competency, so a skill gap and the possible reasons behind it are recorded together.
- Long format matches how real survey platforms store data, and new questions are added as rows, not new columns, so no schema change is needed.
- A separate `assessment_questions` table keeps `assessment_responses` small (IDs and values only) and keeps question wording, type, linked competency and scale in one place.
- Two cycles cost nothing extra structurally and give a real before/after signal. NB03 uses it to measure actual gap closure for KPI K2.2.

**Status:** Accepted

---

## ADR-0019: Three-phase roadmap with reuse-chain principle and upfront data generation

**Context:** The project is planned in phases. The question was how later phases should relate to Phase 1 without redundant rebuilding.

**Options considered:**
1. Separate GitHub repositories, one per phase.
2. One repository, phases with independently generated data.
3. One repository, phases sharing one upfront-generated dataset; later phases reuse earlier schema and data unchanged, adding new methods or filling reserved columns.

**Decision:** Option 3. Phase 1 (Personalized Learning Path Engine) is the built, demoed artifact. All synthetic input data is generated once, upfront, in NB01, including reserved columns (empty in Phase 1), rather than added mid-project.

**Rationale (plain language):**
- Separate repos would multiply documentation and packaging work without adding real value.
- Generating all data upfront avoids regenerating anything later and keeps the whole project reproducible from a single NB01 run (seed 42, per ADR-0013).

**Status:** Accepted

---

## ADR-0020: Scope boundary for later phases

Relates to later phases. Details will be published with that phase.

**Status:** Accepted
---

## ADR-0021: Logic Model adopted as evaluation framework

**Context:** The learner wanted to apply an "operationalization" framework — the kind used by industry practitioners to bridge L&D and business strategy — worked through step by step for learning purposes, rather than simply supplied as a finished artifact.

**Options considered:** Kirkpatrick/Phillips Model, Balanced Scorecard, OKR cascade, Logic Model (Inputs→Activities→Outputs→Outcomes→Impact) — a program-evaluation framework widely used in L&D, nonprofits, and workforce development, and a close cousin of Kirkpatrick's model.

**Decision:** Logic Model, worked through interactively (Inputs → Activities → Outputs → Outcomes → Impact), producing `docs/logic_model.md`.

**Rationale (plain language):**
- Logic Model is the option most explicitly native to L&D practice while still being a standard business-strategy tool — the best fit for a project meant to straddle both worlds, more so than the more purely business-strategy-oriented Balanced Scorecard/OKR options.
- Working through it interactively (rather than receiving a finished mapping) surfaced and corrected several real misclassifications along the way — KPIs and business goals initially miscategorized as Inputs rather than Impact; the competency framework correctly identified as an Input (typically built by an external consultancy/COE, out of scope for this program) rather than an Activity; computed gaps and recommendations initially miscategorized as Outcomes rather than Outputs; "KPI met" initially conflated with individual-level Outcome rather than aggregate Impact. Each correction is preserved in `logic_model.md`'s "Discipline Applied" section as a demonstration of the reasoning process, not just the final answer.
- The exercise revealed and formalized an honest scope boundary: this project builds through the Output stage (tagged content, computed gaps, recommendations) but does not simulate Outcomes or Impact, since doing so honestly would require fabricating longitudinal completion/behavior-change data with no real grounding — consistent with the project's existing discipline (ADR-0002, ADR-0003, ADR-0016) of not fabricating data it can't ground.
- Logic Model complements learning-design models such as ADDIE and COM-B: those answer "how do you design and deliver the learning," Logic Model answers "does this program create value, and how would you tell."

**Status:** Accepted

---

## ADR-0022: Two-stage architecture diagram

**Context:** A visual architecture diagram was identified as the one clear documentation gap against industry-standard portfolio practice — typically the second thing a reviewer looks at, right after a README's opening paragraph.

**Options considered:**
1. Single diagram, drawn once, after the full build.
2. Single diagram, drawn early (before Thread 2's schema work), then never revisited.
3. Two diagrams: a rough conceptual sketch drawn before schema finalization (as a design tool), and a separate polished, GitHub-facing diagram drawn after the build (as documentation).

**Decision:** Option 3.

**Rationale (plain language):**
- Drawing a rough sketch before schema finalization lets the diagram function as a design tool, not just documentation — it forces the whole shape of the system to be visible before table structures are committed, which can surface schema gaps early. This mirrors the conceptual→logical→physical data-modeling discipline discussed earlier in this thread (plain-language entities and relationships, before formal fields/types, before actual tables).
- Waiting until everything is built to draw the only diagram forfeits that early design-check value entirely.
- The final, polished diagram is deferred to the README thread specifically because it should reflect what was actually built, not a draft that may have changed — and it feeds directly into `reports/figures/`, per the folder structure already finalized in ADR-0014.

**Status:** Accepted

## ADR-0023: B13 sketch corrections — TF-IDF as sole production matching pathway

**Context:** Drawing the conceptual architecture sketch (per ADR-0022, before schema formalization) surfaced that the first draft implied all three matching techniques (TF-IDF, supervised classifier, LDA) fed the Path/Recommendation engine. On inspection, only TF-IDF actually does.

**Options considered:**
1. Leave the diagram as-is, with classifier and LDA shown as parallel inputs into the Path engine.
2. Redraw so only TF-IDF flows into the Path engine, with the classifier and LDA shown as side comparison/exploratory boxes connected by dotted leader lines, not arrows.

**Decision:** Option 2. Additionally, resolved four related schema-shaping questions surfaced while redrawing:
- `employee_ksb_gaps` is scoped to proficiency-type assessment responses only.
- `employees_pii` has no foreign-key relationship into any engine table — joined only at the display layer (NB03/NB04), never read by an engine.
- `course_ksb_mapping.match_method` holds `tfidf` (production) or `embedding` (comparison, ADR-0035). The supervised classifier's output is a benchmark comparison against TF-IDF, not a persisted production match.
- LDA topic modeling gets **no schema table**. Its output (topic clusters, keywords) is saved as a notebook artifact/figure (`reports/figures/` or `outputs/`), not written to a database table, since nothing downstream queries it. LDA stays in NB02 (already scoped there via P11) as a clearly marked exploratory section alongside TF-IDF — not split into a separate notebook.

**Rationale (plain language):**
- Mirrors standard practice: analytical output with no downstream consumer belongs as a saved artifact, not a materialized table — bolting exploratory columns onto operational tables (e.g. onto `courses`) is closer to an anti-pattern than a norm in real schema design.
- Keeps the schema honest about what's genuinely production vs. comparison/exploratory, consistent with the project's existing discipline of not fabricating complexity with no real grounding (ADR-0002, ADR-0016).
- Keeps LDA's "secondary lens" framing consistent between the diagram, the schema, and the eventual README narrative — it demonstrates unsupervised technique fluency without pretending it drives the recommendation output.
- NB02's existing scope (P9 TF-IDF, P10 classifier, P11 LDA all in one notebook, per `pipeline_step_tracker.md`) already matched this framing — this ADR formalizes the reasoning rather than changing the notebook plan.

**Status:** Accepted

---

## ADR-0024: Plain-language annotation layer for the architecture diagram

**Context:** The technical architecture sketch (ADR-0022, ADR-0023) is accurate but assumes fluency in ML/data terms. A non-technical reviewer needs a plain-English explanation of what each step actually does.

**Decision:** Each engine box gets a plain-language annotation describing what it does in bullet form. Every bullet is tagged with the phase it belongs to, so the diagram never implies more is built than actually is.

**Rationale (plain language):**
- The diagram should not imply more is built than actually is.
- Tagging every bullet, not just some, avoids a reader guessing whether an untagged line is current or planned.

**Status:** Accepted
---

## ADR-0025: Schema convention set

**Context:** Thread 2 needed to formalize field-level definitions for all 18 tables. Before touching individual tables, six recurring design questions needed a single answer each, since re-deciding them per table would produce an inconsistent schema and slow down NB01 generation logic later.

**Options considered (per convention):**
1. Primary key style — raw auto-increment integers vs. string-prefixed codes.
2. Foreign key naming — match parent PK name exactly vs. allow contextual renaming.
3. Timestamps — generic `created_at`/`updated_at` on every table vs. only where a real downstream use exists.
4. Phase tagging — leave it implicit vs. tag every field explicitly.
5. Reserved columns — add them only when needed vs. type and NULL them now.
6. Categorical fields — plain free-text columns vs. documented enums (full allowed-value list written in the schema doc, since this is pandas/CSV, not a live RDBMS with enforced types).

**Decision:**
1. `<table_singular>_id`, string, prefix code + zero-padded number (e.g. `EMP0001`, `CRS001`, `KSB001`).
2. FK columns always match the parent table's PK column name exactly — no renaming.
3. Skip generic timestamps by default; keep real date fields only where genuinely used downstream (e.g. `assessment_responses.assessment_date`).
4. Fields are tagged `P1` (used in Phase 1) or `P1-reserved` (reserved, empty in Phase 1).
5. Reserved columns are typed and NULL-able from Thread 2 onward, not added mid-project, consistent with ADR-0019's upfront-generation principle.
6. Categorical fields are documented enums: the full allowed-value list is written once in `docs/04_schema_reference.md`, reused identically by NB01 generation code and downstream filtering logic, rather than DB-enforced (not possible in a pandas/CSV project).

**Rationale (plain language):**
- String-prefixed IDs are self-documenting in a raw CSV (`EMP0001` vs. an ambiguous `1`), and are the only thing that keeps the `employees`/`employees_pii` PII-separation design (ADR-0016) safely joinable — both tables must share the exact same ID format for that split to work.
- Consistent FK naming makes every downstream pandas merge a plain `on='employee_id'` — no renaming, no silent mismatches from inconsistent column names.
- Generic timestamps on synthetic, generated-once data would be fabricated theater with no real downstream consumer — skipped on the same "don't fabricate ungrounded complexity" discipline already used in ADR-0002/ADR-0003/ADR-0016.
- Phase tagging at the field level makes clear what is used now and what is reserved, the same discipline as ADR-0010 and ADR-0024.
- Typing reserved columns now (rather than adding them later) means later work fills existing columns instead of altering the schema — no rebuild, matching ADR-0019.
- A documented (not DB-enforced) enum is the practical ceiling available in a pandas/CSV project; writing the full value list once avoids silent typo bugs (e.g. `"Tfidf"` vs. `"tfidf"` producing an empty filter with no error).

**Status:** Accepted

---

## ADR-0026: Two convention exceptions — `proficiency_scale` and `role_competency_requirements`

**Context:** Applying ADR-0025's conventions uniformly surfaced two tables where the standard string-prefixed-PK rule would actively work against the table's actual use.

**Options considered:**
1. Apply the standard string-prefixed PK convention everywhere without exception, for total consistency.
2. Allow narrow, explicitly-documented exceptions where the standard convention would create real downstream cost.

**Decision:** Option 2, with exactly two exceptions:
- `proficiency_scale.proficiency_level` is a plain integer (1–4), not string-prefixed. This value is used in direct arithmetic downstream (`employee_ksb_gaps.gap_size = required_level − actual_level`), which only works cleanly as integer math — a string PK like `LVL03` would force a parse/cast at every gap calculation.
- `role_competency_requirements` has no surrogate primary key at all. Its natural key is the `(role_id, competency_id)` pair — it is a pure junction table (~20 rows: 2 roles × 10 competencies) that nothing else references by an individual row ID. Adding a surrogate `req_id` would be dead weight with no consumer.

**Rationale (plain language):**
- `proficiency_scale`'s levels aren't just labels — they're genuinely ordinal quantities with real arithmetic meaning (the distance between level 2 and level 4 is meaningful), so treating the PK as a plain number rather than a formatted string code respects what the data actually is.
- A junction table's "identity" is fully described by the pair of foreign keys it joins — the only lookup anyone ever performs against `role_competency_requirements` is "give me role X's requirement for competency Y," which the pair already answers without a surrogate ID.
- Both exceptions are narrow and principled (ordinal arithmetic; pure junction table), not convention drift — documenting them explicitly here keeps the schema doc honest about where and why the standard rule doesn't apply, rather than leaving an unexplained inconsistency for a future reviewer to puzzle over.

**Status:** Accepted

---

## ADR-0027: Full 18-table field-level schema closed

**Context:** Following ADR-0025/ADR-0026's conventions, Thread 2 worked cluster by cluster (org/reference, people, assessment, courses, derived) through all 18 tables, producing field-level definitions for every table originally scoped in `config.json`'s Thread 1.5 table list.

**Decision:** Field-level schema finalized for all 18 tables across five clusters, documented in full in `docs/04_schema_reference.md`:
- **Org/reference (10 tables):** `orgs`, `offices`, `job_families`, `roles`, `strategic_goals`, `kpis`, `competencies`, `ksbs`, `proficiency_scale`, `role_competency_requirements`
- **People (2 tables):** `employees`, `employees_pii`
- **Assessment (2 tables):** `assessment_questions`, `assessment_responses`
- **Courses (1 table):** `courses`
- **Derived (3 tables):** `employee_ksb_gaps`, `course_ksb_mapping`, `learning_path_recommendations`

Notable content decisions made while formalizing fields (beyond the conventions in ADR-0025/0026):
- `courses.duration` finalized as `duration_minutes` (int), not free text, so course length can be used directly (for example, summing learning hours per path).
- `courses.credibility_sample_label` added to carry real citation text (e.g. "arXiv:2301.xxxxx — [title], abstract, CC-BY") on the row itself, so provenance for the real-excerpt sample travels with the data rather than living only in README prose — reaffirms arXiv abstracts (not white papers, which are typically still copyrighted despite being freely downloadable) as the real-excerpt source per ADR-0003.
- `assessment_questions.response_scale`: `proficiency`-type questions validate against the existing `proficiency_scale` table (1–4); `comb_*`-type questions use a 1–5 Likert scale documented as free text in this field, with no separate lookup table built, since COM-B responses are collected and held for later use.
- `assessment_responses.assessment_cycle` added as an explicit enum column (`current_year` / `prior_year`) rather than inferring cycle from `assessment_date`, so the two cycles can be compared reliably (used for K2.2 actual gap closure) without date-range logic.
- `employees_pii` name fields finalized as `first_name` (diverse pool, gender + cultural representation) + `last_name` (synthetic 3-letter combo, e.g. "OPQ") + derived `full_name`, and `email` as a synthetic `@lumina-learning.com` domain — explicitly not a real address, to avoid public-repo exposure risk and preserve the project's "honestly synthetic" credibility signal (same discipline as ADR-0002/ADR-0003). A possible self-insert demo row (e.g. `EMP_DEMO`) was discussed and deliberately deferred to NB01 generation — it's a content decision, not a schema one.

**Rationale (plain language):**
- Closing the schema now — rather than leaving fields loosely implied by `config.json`'s table-name list — gives NB01 (Thread 3) a single, unambiguous field-level spec to generate against, with every categorical field's allowed values and every FK's join target already resolved.
- The join map and per-cluster field lists in `docs/04_schema_reference.md` double as the technical backbone for the architecture diagram (ADR-0022/ADR-0023) and will directly support the README's data-flow explanation later (B11).

**Status:** Accepted

---

## ADR-0028: KSB list finalized at 30

**Context:** `config.json` carried a placeholder KSB count ("~29 total, drafted in Thread 1, not yet finalized") pending Thread 2's schema work. The actual Thread 1 draft needed to be located, checked against the 10 finalized competencies, and locked as final content (not just table structure).

**Options considered:**
1. Draft a fresh KSB list from scratch in Thread 2.
2. Retrieve and formalize the Thread 1 draft (2–3 KSBs per competency, Knowledge/Skill/Behavior split) with no content changes, assigning final `ksb_id`s and mapping to `competency_id`s.

**Decision:** Option 2. The Thread 1 draft was retrieved, reviewed, and locked as final with no wording changes: exactly 3 KSBs per competency (Knowledge, Skill, Behavior) across all 10 competencies = **30 KSBs total** (`KSB001`–`KSB030`), not the ~29 placeholder. Full list documented in `docs/04_schema_reference.md`.

**Rationale (plain language):**
- The Thread 1 draft was already well-formed — a consistent 3-per-competency K/S/B split with no gaps or redundant items — so re-drafting from scratch would have added risk (inconsistency with the roles/competencies already built) without adding value.
- Locking at exactly 3-per-competency (rather than the original "2-3" range) removes ambiguity for NB01: every competency generates the same number of KSB rows, keeping `employee_ksb_gaps` generation uniform (each competency-level gap cascades to exactly 3 KSB rows, as noted in `docs/04_schema_reference.md`'s table 16 notes).
- 30 vs. ~29 is a trivial numeric correction, but stated explicitly here so `config.json`'s `ksb_count_target` reads as finalized fact rather than a lingering estimate.

**Status:** Accepted

---

## ADR-0029: `vertical` field added to `employees`

**Context:** While building NB01's Cluster 2 (People), the user shared real career background: at a previous L&D employer, employees were assigned to specific client verticals (e.g., Telecom, Power & Energy), which drove mandatory vertical-onboarding certifications (e.g., Telecom Basics, ERP Basics, Oracle Basics) alongside role-tool certifications (e.g., Articulate Storyline, HTML). The uploaded org chart (`20260907OPModel.xlsx`) already grounds LLG's headcount math in exactly this way — every Content Dev role's row is a per-vertical count across 5 verticals (Pharma, Telecom, Banking/Finance, ERP/Software, Call Center BPO), multiplied out to org totals — but that vertical dimension existed only as a one-time headcount calculation, with no corresponding field on `employees` itself.

**Options considered:**
1. Leave `vertical` out of the schema entirely — treat the org chart's vertical split as a headcount-derivation detail only, not carried into generated data.
2. Add `vertical` as a new field on `employees`, assigning each employee one of the 5 existing LLG client verticals, matching the org chart's own even per-vertical split for each role.

**Decision:** Option 2 — `vertical` (enum: `Pharma` / `Telecom` / `Banking/Finance` / `ERP/Software` / `Call Center BPO`) added to `employees`. Single static value per employee (a point-in-time snapshot, no vertical-transition history table — real-world vertical reassignment over a career, which the user described from experience, is acknowledged but deliberately out of scope, same discipline as not modeling promotion history elsewhere in the schema). This directly informs the `certification_count` generation logic in NB01: all employees (all 4 in-scope roles/career grades, including Solution Architects, who are promoted up from the Instructional Design Manager/CG70 track and so already carry these certs from earlier in their career) get a uniform certification baseline representing vertical-onboarding + role-tool certs, plus a variable count representing DS/ML/GenAI upskilling progress (K2.1).

**Rationale (plain language):**
- The org chart's headcount math already implies every employee belongs to exactly one of the 5 verticals — adding the field makes that implicit structure explicit and queryable, rather than a calculation that's thrown away after generating row counts.
- This is grounded in the user's own real professional experience (vertical-driven mandatory onboarding certs), not fabricated color — consistent with the project's "don't fabricate ungrounded complexity" discipline (ADR-0002/0003/0016), applied here in the positive direction: add realism that IS grounded.
- Scoped deliberately: no vertical-transition history, no 6th vertical beyond LLG's existing 5 (the user's own broader-career vertical, Power & Energy, was explicitly kept out of LLG's scenario as real-world background color, not a scope signal) — keeps the addition small and structural, mirroring how ADR-0025's conventions were applied.
- This is a genuine, if small, schema change from what `04_schema_reference.md` documented at the end of Thread 2 — logged here explicitly (rather than left as silent drift) and reflected in an updated `04_schema_reference.md`.

**Status:** Accepted

---

## ADR-0030: Multi-category learning paths documented as roadmap, not built

**Context:** Following from ADR-0029's vertical-cert background, the user asked whether the project's course catalog/matching engine should also cover domain-onboarding content (e.g., a "Telecom Basics" course, mandatory-for-everyone general business training) alongside the AI/NLP/GenAI competency-upskilling content the system is actually built around.

**Options considered:**
1. Informational only — the fact that employees have vertical/tool onboarding certs sits in `employees.certification_count` (an aggregate int) and now `employees.vertical` (ADR-0029), with no changes to the matching/recommendation engines.
2. A full second, parallel learning-path track — add a `training_category` dimension to the competency/KSB schema (`ai_upskilling` / `vertical_onboarding` / `tool_proficiency` / `general_business`), each with its own courses, KSBs, and rule-based eligibility logic (mandatory-if-vertical-X, mandatory-if-role-Y) running alongside the existing Gap→Match→Recommend engine.

**Decision:** Option 1 for the actual build. Option 2 is documented as a roadmap markdown note only (in NB01 and this ADR) — naming the specific extension mechanism (a `training_category` dimension, rule-based eligibility rather than gap-based proficiency scoring for these categories) without building it.

**Rationale (plain language):**
- The existing TF-IDF matching engine and `course_ksb_mapping`/`employee_ksb_gaps` machinery is built around exactly 30 KSBs under 10 AI/DS/NLP competencies. A course like "Telecom Basics" has zero meaningful keyword overlap with KSBs like "Can prompt-engineer GenAI tools" — domain-onboarding and AI-skill-gap personalization are two genuinely different axes of L&D, not two flavors of the same matching problem.
- Building option 2 properly would mean a second, parallel Gap→Match→Recommend pipeline with its own rule-based (not gap-based) eligibility logic, since domain onboarding is binary done/not-done, not a 1–4 proficiency scale — a real scope expansion, not a tweak.
- This is the same discipline already applied to Project Management (ADR-0004): name the extension specifically enough to show awareness (what dimension, what mechanism, why matching logic mostly carries over but eligibility computation would not), without diluting the project's single, tightly-scoped "one system, one clear job" story.

**Status:** Accepted

---

## ADR-0031: `course_source` field added to `courses` — simulated ingestion layer

**Context:** Continuing the realism discussion started in ADR-0029/0030, the user asked how a real organization's course catalog is actually assembled — confirming their own understanding that course metadata/transcripts from multiple sources (internal LMS, LinkedIn Learning, Coursera, Harvard-style content, etc.) get fed into a matching pipeline against KSB keywords, which is exactly what `courses.description_raw` + the TF-IDF Matching Engine (P9) already do. The question was whether to also simulate the multi-source *ingestion* layer itself, and if so, how much of it.

**Options considered:**
1. **Level A (lightweight):** add a single `course_source` field to the final `courses.csv`, with values naming which platform a course's metadata would have come from — no new files, no transformation logic.
2. **Level B (mini-ETL simulation):** generate separate raw source-format files mimicking each platform's actual export shape (different column names/structures per source), plus a normalization/mapping script that merges them into the unified `courses` schema — a genuine ETL/ingestion-engineering exercise.

**Decision:** Option A (Level A). `course_source` added to `courses`, with 5 values: `Lumina_LMS` (fictional internal LMS — LLG's own authored content), `KonectIn` (fictional stand-in for LinkedIn Learning), `Ksera` (fictional stand-in for Coursera), `Hiword` (fictional stand-in for Harvard-style executive/business content), and `arXiv` (the real-excerpt credibility sample rows, ADR-0003 — a genuinely different kind of source, an open research repository rather than a licensed LXP). Distribution is not random: the 30 AI/NLP/GenAI-specific courses (mapped to the 10 competencies) skew toward `Lumina_LMS` (LLG's own specialized IP), while the 3 generic/orphan courses (Time Management, Meeting Facilitation, Workplace Communication) and the deliberately redundant courses (ADR-0002's built-in overlap, found by duplicate detection, ADR-0041) skew toward or are split across the external platforms — giving a realistic, non-arbitrary reason for that redundancy to exist (two different sources independently covering the same skill, not sloppy internal duplication).

**Rationale (plain language):**
- Level B was rejected because it would mostly mean reformatting the same short synthetic descriptions under different fake column-naming conventions, without a real transcript behind them to justify the extra structure — the project's "don't fabricate ungrounded complexity" discipline (ADR-0002/0003/0016/0030) applies here too: a full multi-format raw-export simulation would be theater, not signal, given that `description_raw` is already an intentional compressed proxy for a full course transcript.
- Level A is genuinely informative, not just decorative: it names *where a course's metadata would have come from* in a real ingestion pipeline, which is exactly the piece of the real-world workflow the user described (multi-source course metadata + transcripts, tagged against KSB keywords) that the schema didn't previously capture.
- Correlating source with content type (internal/specialized → `Lumina_LMS`; generic/soft-skill → external platforms) mirrors real organizational behavior — companies build their own specialized IP but license commodity soft-skill training — and turns the field into a legitimate realism signal rather than an arbitrary label.
- Fictional platform names (rather than the real brand names) avoid implying real-product endorsement or affiliation in a public portfolio piece, while remaining instantly legible as stand-ins.

**Status:** Accepted

---

## ADR-0032: `employees` table scoped to 430, real-org-chart-grounded headcount, not ~1000

**Context:** ADR-0009 committed to generating a master `employees` table "independent of role focus," originally estimated at ~1000 (the org's total headcount) so that future role/job-family extensions wouldn't require regenerating base data. When Thread 3 (NB01) reached actual generation, the user's real org chart (`20260907OPModel.xlsx`) was consulted and found to specify an *exact* headcount for the 4 roles under the project's 2 in-scope job families (JF01 Instructional Design, JF02 Content/Solution Architecture): Content Designer (CG50) — 350, Instructional Design Lead (CG60) — 50, Instructional Design Manager (CG70) — 25, Solution Architect/Capability Development (CG80) — 5. Total: 430. The remaining ~570 of the org chart's 1000 headcount sits in units entirely outside this project's schema (PMO, Graphic Design, Coding, Testing, QA, Innovation Incubator, HR, IT Security, Finance, Business Development, C-Suite).

**Options considered:**
1. Keep `employees` at ~1000 by inventing plausible headcounts for the out-of-schema units (PM, Business Development, etc.) with no real competency/KSB framework behind them.
2. Scope `employees` to exactly 430 — the real, org-chart-grounded headcount for the 2 in-scope job families only — and spin out the broader org-wide roster (all units) as a separate, standalone reusable asset outside this project's schema and CSVs, to be built later if wanted for other portfolio projects.

**Decision:** Option 2. `employees` (and `employees_pii`) generated at exactly 430 rows. The fuller org-wide roster is explicitly deferred as a future, separate asset — not part of `data/raw/`, not tracked in `04_schema_reference.md` or `config.json`'s table list for this project.

**Rationale (plain language):**
- Inventing headcounts for PM/Business Development/etc. (option 1) would mean generating employee rows with no competency requirements, no KSB gaps, and nothing for any engine to compute — orphan rows sitting in the table for no reason other than hitting a round number. This directly conflicts with the project's standing discipline of not fabricating ungrounded complexity (ADR-0002/0003/0016/0030/0031).
- The real org chart is a stronger, more defensible grounding than an arbitrary ~1000 estimate — using its exact per-role, per-vertical headcount numbers (see ADR-0029) is a genuine credibility upgrade over the original placeholder.
- ADR-0009's underlying principle — generate employee data independent of which specific roles get spotlighted in the Phase 1 demo — still holds fully at 430: all 4 roles are populated, not just the 2 `is_phase1_focus_role` roles (Instructional Design Lead, Solution Architect), preserving room to spotlight the other 2 roles later (Content Designer, Instructional Design Manager) without regenerating base data.
- Keeping the broader org-wide roster as a clearly separate, future asset (rather than force-fitting it into this project's schema) respects the same scope-boundary discipline as ADR-0004 (Project Management parked) and ADR-0030 (multi-category paths parked) — named and acknowledged, not built where it doesn't belong.

**Status:** Accepted (supersedes part of ADR-0009 — see that entry's updated status line)

---

## ADR-0033: NB01 Drive-mount cell made idempotent

**Context:** After Thread 3 closed, the user reported that re-running NB01 caused newly-generated CSVs to be created and then immediately trashed in Google Drive, with duplicated folders appearing. Inspecting the actual cell code and its output revealed the cause: NB01's mount cell (`!rm -rf /content/drive` → `drive.mount('/content/drive')`) was being re-run on a Drive that was **already live-mounted** from a prior run in the same Colab session (Colab runtimes commonly persist across "Run All"). The `rm -rf` fix was originally written for a different, narrower situation — a stale/broken local mountpoint from a prior *failed* mount (`ValueError: Mountpoint must not already contain files`) — not for a healthy, already-connected mount. When run against a live mount, `rm -rf` only fails on the handful of protected top-level FUSE roots (`MyDrive`, `.shortcut-targets-by-id`); everything *inside* those (the previous run's `Portfolio/LuminaLearningGroup/data/raw/*.csv`) is deleted through the live Drive connection, and any delete that goes through the Drive-mounted filesystem is routed to Drive's Trash, never a local no-op.

**Options considered:**
1. Keep `!rm -rf /content/drive` as a standing first cell, relying on the user to remember not to re-run NB01 without restarting the runtime first.
2. Make the mount cell idempotent — only wipe/remount when Drive is not already connected — so the cell is always safe to re-run, in every notebook, regardless of runtime state.

**Decision:** Option 2. NB01's first cell (and the first cell of every future notebook — NB02 onward) is:
```python
import os
from google.colab import drive

if not os.path.ismount('/content/drive'):
    drive.mount('/content/drive')
else:
    print("Drive already mounted — skipping remount, nothing deleted.")
```
The original `!rm -rf /content/drive` fix is retained only as manual, one-off troubleshooting guidance (for the specific stale-mountpoint error it was designed for) — never as a standing pipeline cell.

**Rationale (plain language):**
- A destructive command that behaves safely in one specific failure state (stale local mountpoint) and destructively in the normal state (already mounted) has no business being a standing first cell — the fix needed to stop depending on the user remembering the runtime's exact state.
- `os.path.ismount()` is a cheap, side-effect-free check that makes the cell safe to re-run any number of times in any runtime state, which is exactly the property a notebook's first cell should have across a project that will be re-run by a GitHub reviewer, not just the original author.
- This also resolves an integrity risk to the project's own reproducibility story (ADR-0013): silently-trashed data on a routine re-run would have undermined the "anyone can clone and re-run this" claim if it had gone unnoticed longer.

**Status:** Accepted

---

## ADR-0034: Course catalog rebuilt as a procedural generator, scaled to ~150

**Context:** ADR-0002/ADR-0031 established a synthetic course catalog of 36 hand-written courses (plus 4 real-excerpt rows). Reflecting on this against a real employer's course catalog (70,000+ courses), the user pointed out that a hand-typed list of course objects is fundamentally different from how `employees.csv` was built — 430 rows generated from a parameterized `ROLE_SPEC` dict, not typed one by one — and asked why the course catalog wasn't held to the same standard. The concern: a hardcoded list reads as "an adjustment," not a genuine demonstration of dynamic, data-driven generation, which undercuts the project's actual goal of showing how these techniques are used in practice.

**Options considered:**
1. Leave the catalog as 36 hand-written courses — acceptable at small scale, but inconsistent with the `employees.csv` precedent and not a genuine generation exercise.
2. Procedurally generate a much larger catalog (1000 rows), maximizing realism against real enterprise catalog sizes.
3. Procedurally generate a moderate catalog (~150 rows) via a `COURSE_SPEC`-style generator — topic × KSB × provider × phrasing-variant × a controlled duplicate rate × a controlled orphan/noise rate — mirroring the same parametric-generation discipline already used for `employees`.

**Decision:** Option 3. The `courses` table's 36 synthetic rows are replaced by ~150 procedurally-generated rows, produced by a `COURSE_SPEC`-style generator (parameters, not literal course objects). The 4 real-excerpt arXiv placeholder rows (ADR-0003) are unaffected and remain hand-curated, since their entire purpose is to be genuine, individually-sourced real-world text — genuinely different content requires that they stay individually chosen, not generated.

**Rationale (plain language):**
- 1000 rows (option 2) would not teach the downstream matching/dedup/curation work anything that 150 rows doesn't — it adds volume without adding a new kind of signal, and makes the dataset harder to spot-check and explain in a portfolio write-up. This is the same "don't fabricate ungrounded complexity" discipline already applied elsewhere (ADR-0002, ADR-0016, ADR-0030) — scale should be sized to what the demonstration actually needs, not maximized for its own sake.
- ~150 rows is large enough that intentional near-duplicate courses and generic "orphan" courses emerge from randomized generation parameters rather than being manually placed — which matters directly for ADR-0035's duplicate-detection work, since a model finding duplicates in a hand-placed pair proves nothing a human didn't already know.
- This directly fixes the inconsistency the user identified: the catalog is now generated the same way `employees.csv` is — a parameter spec + generation logic, not literal objects — which is itself evidence of the "dynamic technique over hardcoding" principle the project exists to demonstrate.

**Status:** Accepted — supersedes part of ADR-0002 (the catalog is no longer literally hand-authored, though the synthetic-over-scraped decision itself stands) and part of ADR-0031 (the course count changes from ~40 to ~150; the `course_source` field and its 5 values, and the internal-vs-external content-type correlation, are unchanged and still apply to the generator's output).

---

## ADR-0035: Embeddings comparison and duplicate-detection pulled forward into NB02

**Context:** The user reframed the project's actual goal: not just building a working matching engine, but *demonstrating* — in the artifact itself — how ML/DL/NLP solves this L&D business problem in practice, including where AI/ML genuinely speeds things up or improves precision over manual work. Phase 1 had originally planned TF-IDF matching only, with course redundancy detection out of scope.

**Options considered:**
1. TF-IDF only in Phase 1; no redundancy detection.
2. Add an embeddings matcher in NB02 as a real, reported comparison against TF-IDF (precision and compute-cost trade-off), keeping TF-IDF as the production method.
3. Also add course-to-course duplicate detection in NB02, reusing the same embedding vectors — detection only. Deciding which duplicate to retire (disposition) needs usage/cost data this project does not have.

**Decision:** Option 3. NB02 builds, and persists to `course_ksb_mapping`, both `tfidf` (production) and `embedding` (Sentence-BERT/MiniLM, comparison) match rows for each course-KSB pair, with an `is_production` flag marking which one the Path Engine consumes (TF-IDF). NB02 also runs course-to-course similarity clustering over the embedding vectors to populate `courses.likely_duplicate_of`. The classifier and LDA outputs (ADR-0023) remain comparison-only, not persisted. Disposition of duplicates is not built.

**Rationale (plain language):**
- The schema already recorded the matching method per row (ADR-0010), so adding embeddings needed no new structure.
- Persisting both methods' rows (rather than treating embeddings as a notebook-only comparison, the way classifier/LDA are treated per ADR-0023) is deliberate: embeddings need to be a first-class, reportable comparison against the production method, because the point is to show a measurable "here's what AI/ML buys you" number.
- Detection is supported by data the project has; disposition is not, so only detection is built.

**Status:** Accepted — amends ADR-0010 (Phase 1 now includes a real embeddings comparison alongside TF-IDF, not TF-IDF alone).

---

## ADR-0036: Human-in-the-loop curation layer and KPI Impact Rollup added

**Context:** Continuing from ADR-0035's reframing, the user identified a second, equally important story the project should tell: not just what AI/ML does for the organization, but what an L&D professional's judgment adds that a model can't — and how a recommended learning intervention actually traces back to the organization's strategic KPIs (`strategic_goals`/`kpis`, defined since Thread 1 but never consumed by anything downstream). Without an explicit mechanism for either, both claims would only exist as README prose, not as something visible in the pipeline's own output.

**Options considered:**
1. Leave the Matching Engine's output flowing directly into the Path Engine, fully automated, with the L&D professional's role and the KPI connection described only in narrative documentation.
2. Insert an explicit human-review field between matching and path generation, and add a lightweight KPI rollup step tracing recommendations back to `strategic_goals`/`kpis`.

**Decision:** Option 2, two additions:
- **Curation layer:** `course_ksb_mapping` gets a new `curation_status` field (`algo_suggested` / `ld_reviewed` / `ld_approved` / `ld_modified` / `ld_rejected`). NB02 populates every match as `algo_suggested`; only `ld_approved`/`ld_modified` matches are eligible input to NB03's Path Engine. `course_ksb_mapping` also gets `is_recommended` (a ranking/threshold flag over `match_score`, replacing a flat "show every match" output). Exactly how the review transition itself gets simulated (a rule-based stand-in for a real reviewer, versus the user manually annotating a sample) is left as an NB02/NB03 implementation decision, not resolved by this ADR.
- **KPI Impact Rollup:** a new reporting step (NB03 or NB04 — to be settled when that notebook is built) that traces a focus-role employee's closed `employee_ksb_gaps` → their `ld_approved` recommended courses → a projected movement on the relevant `kpis` row (e.g. K2.1, K2.2). No new persisted table is required — it's a computed view over existing tables.

**Rationale (plain language):**
- This makes the "AI proposes, the L&D professional decides" distinction a real, inspectable field in the data — not a claim the README makes on the pipeline's behalf. A reviewer can look at `course_ksb_mapping` and see the review step actually happened, not just read that it should.
- `strategic_goals` and `kpis` have existed in the schema since Thread 1 with no consumer — this finally closes that loop, and does it structurally (a real trace path through existing tables) rather than adding new tables that would need their own justification.
- Consistent with the project's standing privacy/GDPR design (ADR-0016) and swap-point discipline (ADR-0010): a new field on an existing table, not a new engine or new data source, keeps the addition proportionate to what it's demonstrating.

**Status:** Accepted — new addition, not present on any prior version of the architecture diagram. NB02/NB03/NB04 scope updated accordingly in `config.json`.

---

## ADR-0037: `kpi_competency_mapping` added, KPIs typed by lineage, K2.2 reworded to annual cadence

**Context:** Reviewing `strategic_goals.csv`/`kpis.csv`/`competencies.csv`/`ksbs.csv` together (Thread 4, after `kpis.csv`, `competencies.csv`, and `strategic_goals.csv` were added to the project's file set), the user noted that `competencies` → `ksbs` is a clean, joinable chain, but `kpis` → `competencies` has no link at all — the Goal → KPI → Competency → KSB traceability chain that ADR-0036's KPI Impact Rollup implicitly depends on was never actually built as a real join.

**Options considered:**
1. Add a single `competency_id` FK directly on `kpis` — simplest, but forces a false 1:1 KPI→competency relationship.
2. Add a `kpi_competency_mapping` junction table (many-to-many), mirroring `role_competency_requirements`'s no-surrogate-PK convention (ADR-0026), with an added `contribution_weight` field so competencies that only marginally relate to a KPI aren't weighted the same as the ones the KPI is actually about.
3. Before building either: check whether every KPI genuinely maps to competencies at all, rather than assuming all 4 do.

**Decision:** Option 3 first, which changed the shape of options 1/2. Checking each KPI against what it actually measures surfaced that the 4 KPIs are not structurally alike:
- **K1.1 / K1.2** (program/client-level: % of client programs using AI-personalized paths; client satisfaction score) have no individual-employee competency signal behind them at all — in a real org these are tracked externally (client surveys, deployment records), not derived from competency data. Mapping them to competencies would be fabricating a link that doesn't exist.
- **K2.1** (% of Content Dev staff certified in DS+ML fundamentals) is genuinely competency-driven, and unevenly so — some competencies are core to it, some only tangential.
- **K2.2** (KSB gap-closure rate) is a computed aggregate that spans *whichever* competencies apply per employee by construction — mapping it to specific competencies would be meaningless, since it isn't about any one of them.

So: added `kpi_type` to `kpis.csv` (`qualitative_program_level` for K1.1/K1.2, `computed_competency_driven` for K2.1, `computed_aggregate` for K2.2) to make this lineage difference explicit rather than leaving a reader to infer it from which rows appear in the mapping table. Added `kpi_competency_mapping.csv` (`kpi_id`, `competency_id`, `contribution_weight` — `primary`/`secondary`), populated for **K2.1 only** (5 rows: COMP01/COMP02/COMP07 primary, COMP03/COMP04 secondary). K1.1/K1.2/K2.2 are deliberately left out of the mapping table.

Separately, working through K2.2's actual computation (gap-closure per employee, from `employee_ksb_gaps` across the two assessment cycles) surfaced that its original wording — "1.5 KSBs/quarter" — doesn't match the data: `assessment_responses` only has two cycles per *year* (`prior_year`/`current_year`, per ADR-0018), not per quarter. Rather than add a quarterly assessment cadence (which would ripple into ADR-0018's locked two-cycle design, and isn't realistic anyway — quarterly formal competency reassessment is uncommon in real orgs), K2.2's `kpi_text` and `target_value` in `kpis.csv` were reworded to an annual cadence: *"Average KSB gap-closure rate per employee per assessment cycle (annual)"*, target `6 KSBs/year` (the same ambition as the original 1.5/quarter, restated at the cadence the data actually supports).

**Rationale (plain language):**
- Checking whether each KPI genuinely traces to competencies before building the join table avoided a bigger mistake — force-fitting all 4 KPIs through one mechanism would have either fabricated a link for K1.1/K1.2 that has no real basis, or diluted K2.2's meaning by assigning it arbitrary competency weights. This is the same "don't fabricate ungrounded complexity" discipline used throughout the project (ADR-0002, ADR-0016, ADR-0030), applied to a KPI schema instead of a data-generation choice.
- `contribution_weight` (primary/secondary, not a numeric score) mirrors how a real BA/L&D team would actually describe a KPI's competency drivers — some competencies obviously matter more than others, and pretending they're all equal would misrepresent the KPI.
- The junction-table shape (option 2, not option 1) follows the same precedent as `role_competency_requirements` (ADR-0026) — a natural many-to-many relationship, not forced into a false one-to-one.
- Rewording K2.2 rather than adding a quarterly assessment cycle keeps the fix scoped to the KPI's own wording (an error in the KPI, not the data model) instead of rippling into a locked, purpose-built design decision (ADR-0018).

**Status:** Accepted

---

## ADR-0038: Course target labels persisted in `courses` at generation time

**Context:** NB01's course generator knows, for every synthetic course, which KSB and competency it was written to teach. That information was discarded when `courses.csv` was saved. NB02 needed it for the supervised classifier (a label per course) and for the "did the matcher find the intended KSB" check. Without it, those checks would have had to guess labels from the same text being tested.

**Options considered:**
1. Re-derive labels in NB02 from the matching output (circular: the test would grade itself).
2. Persist the generator's own intent as columns in `courses.csv`, then re-run NB01.

**Decision:** Option 2. `courses` gets `target_ksb_id` and `target_competency_id` (nullable). 24 rows are NULL: 20 orphan/generic courses and 4 real arXiv rows. They sit outside the taxonomy by design and are excluded from supervised work. NB01 was re-run once; seed 42 reproduced identical content.

**Rationale (plain language):**
- A test needs an answer key written independently of the thing being tested. The generator's intent is that key.
- Orphans and real excerpts stay NULL on purpose. Inventing labels for them would be fabricated ground truth.
- Real catalogs have no such column. In production, labels come from L&D tagging or past enrolment. This is a synthetic-data advantage and is stated as a limitation.

**Status:** Accepted

---

## ADR-0039: Supervised classifier benchmark is non-discriminating on synthetic data

**Context:** ADR-0017 planned a course→competency classifier benchmarked against TF-IDF matching. In NB02 Step 2.7, a Logistic Regression (TF-IDF features fit on train only, stratified 70/30 split, 39 test courses) was compared with TF-IDF similarity.

**Options considered:**
1. Report the classifier's accuracy alone.
2. Compare it fairly with TF-IDF on the same 39 test courses: for each course, take the best-scoring KSB across all 30, map it to its competency, and compare with `target_competency_id`.
3. Change the generator to add noise so the two methods separate.

**Decision:** Option 2, no generator change. Result: Logistic Regression 39/39, TF-IDF 39/39. The benchmark does not separate the two methods. A first TF-IDF comparison that used only `is_recommended` rows covered 6 of 39 test courses and was replaced by the full-score comparison. The classifier is comparison-only and is not persisted to `course_ksb_mapping` (ADR-0023).

**Rationale (plain language):**
- Template-generated course text gives each competency its own vocabulary, so any reasonable method finds the answer. Perfect scores say the data is easy, not that the models are equal.
- Making the data noisier just to get a spread would be tuning the test to produce a story. Reporting the tie honestly is the stronger signal.
- What would separate them in real life: paraphrased descriptions, vague titles, overlapping topics, and a labelled catalog large enough to train on. Mitigations: human review of low-confidence matches, fallback to keyword rules, retraining as labels accumulate.

**Status:** Accepted

---

## ADR-0040: LDA topic count chosen by coherence, after data-driven filler-word removal

**Context:** Step 2.8 runs LDA on course text as an exploratory lens (ADR-0017, ADR-0023). A first run with k=10 produced topics made of template phrases ("projects", "real", "practical"), and perplexity gave no elbow. Coherence on that run was inflated by the repeated phrases.

**Options considered:**
1. Keep k=10 because there are 10 competencies (design-driven).
2. Remove the filler words, then choose k by c_v coherence (gensim) over k=5 to 15.
3. Rewrite course text to be less templated.

**Decision:** Option 2. Filler words were detected from the data: any word appearing in 8 or more of the 10 competencies (33 words). The vectorizer excludes these plus English stop words (min_df=2, max_df=0.9). Coherence peaked at k=12 (0.689), so k=12 was used (random_state=42, batch learning). The cross-tab against `target_competency_id` shows partial recovery: COMP05 and COMP10 are captured cleanly; COMP04, COMP05 and COMP09 share one topic. Outputs saved to `outputs/lda_topic_words.csv` and `outputs/lda_topic_vs_competency.csv`. LDA stays exploratory and is not persisted to any table.

**Rationale (plain language):**
- k should come from the data. Matching k to the competency count would assume the answer.
- Filler removal is a normal step: real corpora have boilerplate such as "this course covers" that is removed the same way.
- **Limitation:** competency labels were used to find the filler words, so the cross-tab is less independent than a fully unsupervised check. In real use, document frequency alone would be used.
- Topic names in the notebook are interpretations, confirmed or corrected by the cross-tab, not facts.

**Status:** Accepted

---

## ADR-0041: Duplicate detection by embedding similarity threshold and connected components

**Context:** ADR-0035 pulled duplicate detection forward into NB02 (detection only).

**Options considered:**
1. Flag pairs above a round threshold chosen in advance.
2. Choose the threshold from the similarity score distribution, then group flagged pairs with connected components (networkx) so chains of duplicates form one cluster.

**Decision:** Option 2. Threshold 0.95, chosen from the distribution. Result: 12 clusters covering 27 courses. `courses.likely_duplicate_of` points to the cluster's representative course and `courses.duplicate_cluster_id` (`DUPCLUST###`, nullable) names the cluster. Nothing is dropped or merged. Which course to retire (disposition) is not decided here, since it needs usage and cost data this project does not have.

**Rationale (plain language):**
- Pairs alone hide groups: if A≈B and B≈C, they are one family. Connected components capture that.
- The 0.95 cutoff is a judgment call read off the distribution, not a tuned optimum. A stricter cutoff finds fewer, surer duplicates; a looser one finds more false ones.
- Detection without disposition keeps the project inside what its data supports.

**Status:** Accepted

---

## ADR-0042: Rule-based curation stand-in, with an open resolution question

**Context:** ADR-0036 added `curation_status` and left open how the review step is simulated. NB02 Step 2.6 needed a first answer.

**Options considered:**
1. Mark everything `algo_suggested` and leave review entirely to NB03.
2. A rule using the two matchers: for each recommended TF-IDF match, if TF-IDF and MiniLM agree on the top course for that KSB and the score gap between them is under 0.10, set `ld_approved`; otherwise `ld_reviewed`.
3. Hand-annotate a sample.

**Decision:** Option 2. `CONFIDENCE_GAP_THRESHOLD = 0.10`. Result: 11 `ld_approved`, 19 `ld_reviewed` (30 recommended TF-IDF matches, one per KSB). Step 2.7.6 showed all 30 top matches are correct for both methods against `target_ksb_id`, so the 0.10 cutoff marks low agreement between methods, not wrong answers. The cutoff was not tuned from data. `ld_modified` and `ld_rejected` are unused in NB02.

**Open item (carried to NB03):** decide how the 19 `ld_reviewed` matches become approved, modified or rejected, before they can feed the Path Engine, which accepts only `ld_approved`/`ld_modified`.

**Rationale (plain language):**
- Agreement between two different methods is a cheap, honest proxy for confidence. Disagreement is where a human reviewer's time is best spent.
- This is a stand-in for a reviewer, not a reviewer. The README must say so.
- Leaving 19 rows unresolved is deliberate: forcing them to `ld_approved` would hide the human step this layer exists to show.

**Status:** Accepted (open item carried to NB03; resolved by ADR-0043)

---

## Note: Phase 1 engines updated (Thread 4 kickoff)

Following ADR-0033 through ADR-0036, the Phase 1 engines are updated as follows:

| Engine | Status | What changed |
|---|---|---|
| Diagnostic Engine | Unchanged | Still assessment rows → gaps |
| Course-KSB Matching Engine | TF-IDF unchanged; embeddings added | TF-IDF remains production; embeddings comparison built in NB02 (ADR-0035) |
| **Duplicate detection** | **New** | Course-to-course clustering in NB02, detection only (ADR-0035, ADR-0041) |
| **L&D Curation Review** | **New** | Inserted between Matching Engine and Path Engine — human approves/modifies/rejects before a match becomes a recommendation (ADR-0036) |
| Path Engine | Unchanged in concept, changed input | Now consumes only `ld_approved`/`ld_modified` matches, not raw model output |
| **KPI Impact Rollup** | **New** | Traces an approved recommendation back to `strategic_goals`/`kpis` (ADR-0036), likely NB03 or NB04 |

## Note: documentation_tracking.md and master_prompt.md status at Thread 2 close

`docs/documentation_tracking.md` was not accessible in Thread 2's file context and was not edited. It requires one manual addition: a row for `docs/04_schema_reference.md` (new artifact from this thread — finalized field-level schema for all 18 tables, six schema conventions, and the 30-item KSB list; see ADR-0025 through ADR-0028).

`master_prompt.md` **was** updated in this thread — "Current status" section rewritten for Thread 2 completion/Thread 3 handoff, and the "Files to reference" section updated to add `docs/04_schema_reference.md` and remove the stale `01_data_schema.md` reference (confirmed in this thread to never have actually existed in the project knowledge base). No outstanding action needed on `master_prompt.md`.

## Note: Thread 3 (NB01) close — outstanding manual action

Thread 3 (NB01 — Synthetic Data Generation) is complete: all 15 input tables generated cluster-by-cluster (Org/Reference, People, Assessment, Courses) in a single Colab notebook, per ADR-0029 through ADR-0032. `docs/04_schema_reference.md` and `config/config.json` updated to reflect the `vertical` and `course_source` field additions and the corrected 430-employee scope.

**Outstanding manual action, before NB02 runs:** `courses.csv` rows `CRS037`–`CRS040` are placeholders (`is_real_excerpt = True`, `course_source = "arXiv"`). The user must manually paste in the actual abstract text and finalize the citation for 4 identified candidate papers (title + arXiv ID only supplied by Claude; full abstract text intentionally not reproduced due to copyright — see ADR-0003):
- CRS037 → arXiv:2508.19569 — Skill-based Explanations for Serendipitous Course Recommendation
- CRS038 → arXiv:2505.02324 — From Course to Skill: Evaluating LLM Performance in Curricular Analytics
- CRS039 → arXiv:2510.05495 — Automated Research Article Classification and Recommendation Using NLP and ML
- CRS040 → arXiv:2007.11053 — Curriculum Vitae Recommendation

**Update (Thread 4):** CRS037–CRS040 (renumbered CRS151–CRS154 after the P5 procedural-generator rebuild, ADR-0034) have since been completed — real abstract text and citations pasted in by the user.

## Note: decisions.md restored after accidental deletion (Thread 3)

`decisions.md`, `config.json`, and `04_schema_reference.md` were briefly deleted from the project knowledge base by the user (intending to replace with downloaded copies) and restored from this session's own working context immediately after. Content is identical to the version finalized moments earlier at Thread 3 close — no information was lost.

## Note: Thread 4 (NB02) close

NB02 is complete (P7 to P11, plus P9a, P9b, and P9c as a rule). ADR-0038 to ADR-0042 record the decisions made while building it; total ADRs: 42.

Schema changes from NB02, logged in `04_schema_reference.md`: `courses.target_ksb_id`, `courses.target_competency_id`, `courses.duplicate_cluster_id`; `course_ksb_mapping.is_production`/`is_recommended`/`curation_status` populated (30 recommended per method); `employee_ksb_gaps.gap_size` range −2 to +2; `kpis.kpi_type`; `kpi_competency_mapping` table. The `course_text_feature` column built in NB02 is a helper only and is not part of the schema.

Outstanding for NB03: resolve the 19 `ld_reviewed` matches (ADR-0042); decide the role of `courses.ksb_layer`; decide whether duplicate clusters limit recommendations; settle the home of the KPI Impact Rollup.

Process note: `courses.csv` edited by hand must be saved as CSV UTF-8, otherwise `pandas.read_csv` raises `UnicodeDecodeError`. `requirements.txt` for GitHub must include sentence-transformers, networkx and gensim.

---

## ADR-0043: L&D curation resolved through a blind side-by-side review sheet

**Context:** ADR-0042 left 19 `ld_reviewed` TF-IDF matches unresolved. The Path Engine accepts only `ld_approved`/`ld_modified` rows. NB02 had already shown all 19 were correct matches, so the decision was a choice among valid courses.

**Options considered:**
1. A rule using `target_ksb_id` to auto-approve (breaks ADR-0038: target labels are for benchmarking only).
2. Force all 19 to `ld_approved` (hides the human step this layer exists to show).
3. Export a side-by-side review sheet, have the L&D reviewer decide, and load the decisions back.

**Decision:** Option 3 (NB03 Step 3.2).
- Sheet: one row per KSB with the suggested course (TF-IDF top) and the alternative (MiniLM top): title, description, provider (`course_source`), format, duration, `ksb_layer`, duplicate cluster, score. `target_ksb_id` hidden (blind review).
- Decisions: `approve` (keep suggested), `switch` (use alternative), `reject` + `replacement_course_id` (reviewer picks another course), `reject` with no replacement (content gap). Every reject or switch carries a `reviewer_note`.
- Mapping: approve → `ld_approved`; switch or reject-with-replacement → chosen row `ld_modified` and the algorithm's pick `ld_rejected`; reject without replacement → `ld_rejected`.
- Result: 12 approve, 4 switch (KSB010, KSB014, KSB015, KSB029), 2 replace (KSB019 → CRS085, KSB025 → CRS114), 1 content gap (KSB018). TF-IDF statuses: 23 `ld_approved`, 6 `ld_modified`, 7 `ld_rejected`. 29 of 30 KSBs have exactly one eligible course.
- Audit trail: `outputs/curation_review_sheet.csv` (export) and `outputs/curation_review_sheet_completed.csv` (decisions, hand-edited, CSV UTF-8). Both retained.

**Rationale (plain language):**
- This is how real L&D teams curate AI suggestions: a reviewer sees the options and decides, and the reason is logged.
- Switches mostly favoured practice and engagement over passive formats. That is L&D judgment the algorithm cannot supply.
- No automatic "next best" replacement: anything that reaches a learner has been seen by a human.
- The review also surfaced a catalog issue: two different courses (CRS083, CRS089) share one title. A real curator would flag this.

**Status:** Accepted (resolves the ADR-0042 open item)

---

## ADR-0044: Content gaps shown as "coming soon" path steps

**Context:** KSB018 (proactively manages client expectations on AI content accuracy, a behavior) has no suitable course. The reviewer judged it needs a workshop or simulation.

**Options considered:**
1. Drop the gap from the path silently.
2. Auto-assign the next-best course.
3. Keep a path step with no course and a clear message, and log the gap for build or buy.

**Decision:** Option 3.
- `learning_path_recommendations.course_id` is nullable, for content-gap rows only (19 rows, all KSB018).
- The message lives in one editable setting (`CONTENT_GAP_MESSAGES` in Step 3.4): "New workshop/simulation planned for Q1 2027. Meanwhile, use your manager 1:1s or the mentor-mentee programme to build this skill."
- Content-gap steps sit at the end of the path and count as "pending content" in the KPI rollup, not as gaps addressed.

**Rationale (plain language):**
- Learners should see that a need is known and being handled, not that it vanished.
- Interim routes (manager 1:1s, shadowing, mentoring) reflect real practice for behavior needs.
- An unfilled need is a useful business signal: content to build or buy.

**Status:** Accepted

---

## ADR-0045: Path sequencing rules

**Context:** NB03 Step 3.4 had to turn each employee's open gaps (`gap_size > 0`) plus one approved course per KSB into an ordered path. Open items from NB02 also needed closing: the role of `courses.ksb_layer`, and whether duplicate clusters limit recommendations.

**Decision:** Rule-based sequencing (`recommendation_method = rule_based`):
1. Courses first; content-gap steps last.
2. Biggest gap first (3, then 2, then 1).
3. Within a competency: Knowledge, then Skill, then Behavior, using `ksbs.ksb_type`.
4. **Shared course:** if one course serves two KSBs for an employee, both rows share one `sequence_order` and the rationale says "Also covers ...". One row per open gap is kept, so every gap stays traceable via `gap_id`.
5. **Duplicate check:** if two courses in one path share a `duplicate_cluster_id`, keep the earlier one. Result: 0 swaps needed.
6. **`ksb_layer` is a fit check only:** mismatches (for example a skill-layer course for a knowledge need) are noted in the rationale. It does not reorder or drop courses.

Result: 687 rows for 55 employees; paths of 5 to 21 steps (mean 11.6); 98 rows use a shared course (CRS085, CRS114); 19 content-gap rows.

**Rationale (plain language):**
- Biggest-need-first and K-S-B order follow how real curricula build capability.
- A learner should never be asked to take the same course twice, or two near-copies.
- `ksb_type` already gives the K-S-B order, so `ksb_layer` is better used to flag poor fit.
- Behavior needs are flagged for on-the-job application, since no course alone builds a behavior. In COM-B terms, behavior also needs opportunity and motivation, not only capability.

**Status:** Accepted

---

## ADR-0046: KPI Impact Rollup computed in NB03 with two projections

**Context:** ADR-0036 left the home of the KPI Impact Rollup open (NB03 or NB04).

**Decision:** Compute in NB03 (Step 3.6); chart in NB04. Aggregate rows only, saved to `outputs/kpi_impact_rollup.csv` (no new table).
- **K2.2 actual:** KSBs closed per employee between `prior_year` and `current_year` (a competency gap that went from positive to zero or below, × 3 KSBs). Result: 3.2 vs a target of 6.
- **K2.2 projected:** open gaps each path addresses. Optimistic (all gaps with a course close) 12.1; conservative (only 1-level gaps close in one cycle) 9.6.
- **K2.1 proxy:** % of focus-role employees meeting the requirement on all 3 primary K2.1 competencies (COMP01, COMP02, COMP07). Now 9.1%; conservative 70.9%; optimistic 100%. Target 60%.
- K1.1/K1.2 out of scope (`qualitative_program_level`, ADR-0037).

**Rationale (plain language):**
- The data lives in NB03; NB04 is for presenting it.
- Two projections avoid overselling one number.
- **Completion is not competence.** Real gap closure needs reassessment, manager feedback and on-the-job evidence. Projections are best case, consistent with the Logic Model stopping at Outputs (ADR-0021).
- Projected K2.2 is about twice the annual target, so paths should be phased over about 2 cycles. This is a real L&D workload insight.
- Limits: K2.1 here covers the 55 focus-role employees, not all Content Dev staff, and uses meeting the competency requirement as a stand-in for certification.

**Status:** Accepted (Thread 6: the "about 2 cycles" average was refined per person by ADR-0050)

---

## ADR-0047: Source-of-truth fixes in NB01 and re-run guards

**Context:** NB03 Step 3.1 found `kpis.csv` on Drive had reverted to the pre-ADR-0037 version (no `kpi_type`, K2.2 still "1.5 KSBs/quarter"). Cause: the ADR-0037 fix had been made by hand in the CSV, and the ADR-0038 re-run of NB01 regenerated the file from old code. Separately, employee IDs were found to be assigned role by role (ID Leads EMP0001-0050, and so on), unrelated to tenure, which is unrealistic.

**Decision:** Fix at the source, regenerate, and add guards.
1. **KPIs:** NB01 C1.6 now builds `kpis` with `kpi_type` and the annual K2.2 wording; new C1.6a generates `kpi_competency_mapping` (previously hand-made). C1.11 saves 11 tables; an assert check stops silent reverts.
2. **Employee IDs:** NB01 C2.3a re-keys `employee_id` by joining order: hire year = 2026 − tenure, random order within a year (seed 42), IDs drawn from a 1-1,400 pool with gaps (other departments and leavers). Low IDs = longest tenure; roles interleaved.
3. **Cascade re-run:** NB01 Clusters 2 and 3; NB02 Steps 2.1.1-2.1.3; NB03. Course matching and curation are unaffected (no employee IDs). Gaps were re-drawn: still 1,650 rows; `gap_size` range now −2 to +3; 687 open gaps (540 × 1, 144 × 2, 3 × 3).
4. **Guards:** NB01 Cluster 4 save is behind `REGENERATE_COURSES = False` (protects hand-pasted arXiv abstracts and NB02 columns; still writes on a fresh setup). NB03 Step 3.2.1 exports the review sheet only when `ld_reviewed` rows exist. NB01 and NB03 are now safe to run end to end.

**Rationale (plain language):**
- Any file a pipeline generates is overwritten on the next run, so fixes belong in code, not in the output. This is standard practice.
- IDs that follow joining order match real HR systems and stop IDs clashing with tenure.
- Guards make re-runs safe for a GitHub reviewer, not just the author.

**Status:** Accepted (amends ADR-0032's ID range and ADR-0037's implementation)

---

## ADR-0048: Privacy demo and "GDPR-aligned" wording

**Context:** ADR-0016 committed to a per-employee access pattern. NB03 Step 3.5 built it.

**Decision:**
- `view_my_path(logged_in_id, requested_id)` shows a path only to its owner; any other request is refused. The name is joined from `employees_pii` only at this display step.
- Each step appears once (a shared course shows once); content gaps show as "Coming soon".
- Wording: describe the design as **"GDPR-aligned privacy by design"**, never "GDPR compliant". A markdown cell after Step 3.5 lists what is built (PII split, own-data access, aggregate-only reporting, human review before recommendations reach learners) and what is roadmap only (minimisation audit, encryption, consent, erasure, retention, real authentication).

**Rationale (plain language):**
- Compliance is a legal status that needs real data and legal review; a synthetic portfolio can only show the pattern. Saying so is honest.
- Human review before recommendations reach learners also speaks to GDPR's rules on purely automated decisions.

**Status:** Accepted (extends ADR-0016)

---

## ADR-0049: CC BY-NC 4.0 licence replaces planned MIT

**Context:** ADR-0014 planned an MIT `LICENSE`. MIT lets anyone reuse everything, including commercially. The user wants to publish the full repo (including NB01, for reproducibility and credibility) but not let others reuse the hand-built datasets commercially.

**Options considered:**
1. MIT (fully open).
2. Leave NB01 out of the repo (weakens reproducibility and credibility; the CSVs could still be copied).
3. CC BY-NC 4.0: others may view and learn, must credit the author, may not use commercially.
4. No licence (all rights reserved by default).

**Decision:** Option 3. Publish everything, including NB01, under CC BY-NC 4.0.

**Rationale (plain language):**
- A licence, not hiding files, is the real protection.
- Keeps the project fully reproducible and reviewable for recruiters.
- Caveats: a public repo can still be copied in practice; the licence gives legal standing, not technical locks. Creative Commons does not recommend CC licences for software code, so at GitHub packaging the licence was split: code under PolyForm Noncommercial 1.0.0 (`LICENSE`), data, docs and figures under CC BY-NC 4.0 (`LICENSE-DATA-DOCS`). Both require credit and forbid commercial use. Not legal advice.

**Status:** Accepted (supersedes the MIT choice in ADR-0014; code licence settled at GitHub packaging)

---

## Note: Thread 5 (NB03) close

NB03 is complete: P12, P12a (KPI rollup, NB03 per ADR-0046) and P13 done. ADR-0043 to ADR-0049 added; total ADRs: 49.

Open items from NB02 resolved: 19 `ld_reviewed` (ADR-0043); `ksb_layer` role (ADR-0045); duplicate clusters (ADR-0045, 0 swaps); KPI rollup home (ADR-0046).

Schema changes (logged in `04_schema_reference.md`): `learning_path_recommendations` created (687 rows, `course_id` nullable for content gaps, shared `sequence_order`); `employees.employee_id` re-keyed (non-contiguous, EMP0003-EMP1399); `employee_ksb_gaps.gap_size` range −2 to +3; `course_ksb_mapping.curation_status` now uses all five values; `kpis` and `kpi_competency_mapping` generated in NB01 code.

New `outputs/` artifacts: `curation_review_sheet.csv`, `curation_review_sheet_completed.csv`, `kpi_impact_rollup.csv`.

Carried to NB04: chart two contrasting paths (ID Lead vs Solution Architect); KPI rollup dashboard (aggregate only); show the human review step and the KSB018 content gap visually; consider phasing paths over 2 cycles.

---

## ADR-0050: Path phasing over annual cycles, with two caps

**Context:** NB03 projected 9.6 to 12.1 KSBs closed per employee, about twice the K2.2 target of 6 a year. Paths were too long for one year, so NB04 needed a phasing rule grounded in real practice, not invented.

**Options considered:**
1. KSB cap only: 6 KSBs per cycle (the K2.2 target).
2. Hours cap only: 40 learning hours a year minus 5 hours of mandatory, compliance and QMS training = 35 hours for the path (the user's industry experience: compliance modules are assigned one at a time, max 3 a year, 45 to 60 minutes each, mostly in Q3; the rest is QMS).
3. Both caps; a new cycle starts when the next step would break either one.
4. Add a practice-time multiplier to convert seat time into total effort.

**Decision:** Option 3 (NB04 Step 4.3).
- Settings at the top of NB04: `ANNUAL_LEARNING_HOURS = 40`, `MANDATORY_HOURS = 5`, `KSB_PER_CYCLE = 6`.
- Steps stay in path order. A shared course counts as 2 KSBs. "Coming soon" steps have no course, so they are marked pending, not placed in a cycle.
- Display only: cycles are computed in memory (`phased`) and not saved. No schema change.
- Result: 638 path steps. 8 employees need 1 cycle, 22 need 2, 25 need 3 or more (reported as a top band, ADR-0055). The KSB cap set all 73 cycle breaks. The hours cap was never reached: mean path 6.6 course hours, at most 5.9 hours in any cycle.

**Rationale (plain language):**
- The 40-hour budget and the mandatory share come from real practice, so the rule is grounded.
- Course minutes are seat time only. Practice, assignments and on-the-job application are not in the data, and no grounded multiplier exists, so option 4 was not used.
- Finding: time is not the limit; how much change a person can absorb in a year is. The NB03 average ("about 2 cycles") hid that almost half of employees need 3 or more years.
- In real life L&D would prioritise role-critical competencies first. Not built; stated in the notes.

**Status:** Accepted

---

## ADR-0051: NB04 figure conventions

**Context:** NB04 produces the images the README will embed. GitHub shows saved images, not live plots, and every figure must look like part of one system.

**Options considered:**
1. Interactive Plotly charts (richer, adds a package, does not render on GitHub).
2. Static matplotlib PNGs with one shared style.

**Decision:** Option 2.
- matplotlib only; NB04 adds no new packages.
- PNG at 150 dpi in `reports/figures/`, named by notebook step: `fig_4_2_curation_flow.png`, `fig_4_3_cycles_needed.png`, `fig_4_4_paths_idlead_vs_sa.png`, `fig_4_5_kpi_dashboard.png`.
- One colour-blind-safe palette: Knowledge blue, Skill orange, Behavior green; content gap hatched grey; target lines dashed vermillion; projections in a lighter shade.
- Every number in a figure is read from the data, with assert checks; nothing is typed in.
- Curation flow drawn as a box flow, not a Sankey. A Sankey (Plotly) is noted as a more visual option.
- Projections carry a best-case footnote: completion is not competence (ADR-0046).
- Step 4.6 checks that all 4 figures exist (42 to 219 KB each).

**Rationale (plain language):**
- One static style reads as one product and loads anywhere.
- Step-numbered file names let a reader trace any picture back to its code.
- Reading numbers from data means figures stay correct if the pipeline is re-run.

**Status:** Accepted

---

## ADR-0052: Demo employee selection and the privacy gate on charts

**Context:** P15 needs two contrasting example paths. In real life nobody is "picked": every employee sees their own path. The demo must not imply otherwise, and charts must respect the same privacy rule as NB03.

**Decision:**
- **Selection rule (demo only):** per role, the employee whose path length is closest to the role median; the ID Lead must also have the KSB018 "coming soon" step. Result: EMP0476 (Instructional Design Lead, 12 steps, 6.5 course hours, 2 cycles) and EMP0188 (Solution Architect, 8 steps, 6.8 course hours, 2 cycles).
- **Privacy gate:** `plot_path()` calls `can_view(logged_in_id, requested_id)`, the same rule as NB03 `view_my_path`. Step 4.4.5 shows a request for a colleague's path being refused.
- **Data minimisation:** NB04 does not load `employees_pii`. Figures show employee IDs only.
- **Real-life note in NB04:** selection code is removed; employees see their path on Workday or a similar platform, signed in through Single Sign-On, which supplies `logged_in_id`; managers see team summaries; L&D and leaders see totals only; stakeholder examples use anonymised personas.

**Rationale (plain language):**
- A "typical" example is more credible than a cherry-picked one, and choosing one with KSB018 shows the content gap without an extra chart.
- A chart is just another view of personal data, so it uses the same gate. One rule, reused.
- Described as GDPR-aligned, not compliant (ADR-0048).

**Status:** Accepted (extends ADR-0016, ADR-0048)

---

## ADR-0053: Version 2 backlog kept as a private file

**Context:** NB04 discussions raised good ideas that the current data cannot support.

**Options considered:**
1. Build them now (needs fabricated data and new tables).
2. Add them to the public roadmap.
3. Keep a private backlog file for a future version.

**Decision:** Option 3. The backlog is kept outside Drive and GitHub. Each item records why it matters and why not now.

**Rationale (plain language):**
- Ideas are not lost, and the public repo stays honest about what is built.

**Status:** Accepted

---

## ADR-0054: Two-layer notes ("plain line, then Under the hood")

**Context:** Notes must work for a layman (recruiter, L&D leader) and for a technical reader, without two separate documents.

**Decision:**
- Notebook markdown leads with a plain-language line. Where useful, an indented *Under the hood* sub-bullet names the function, file or rule.
- Notes/Insights use short, everyday words.
- README uses the plain layer only, linking to notebooks for detail. Code comments stay technical.
- From NB04 Step 4.2 onwards, every comment block in a code cell carries its substep number (Step 4.x.y).

**Rationale (plain language):**
- A layman reads the first line and stops; a technical reader continues. Both are served on the same page.

**Status:** Accepted

---

## ADR-0055: Minimum group size for aggregate views

**Context:** NB04 reports totals only (ADR-0016, ADR-0048). But some groups are very small. There are only 5 Solution Architects (the real org-chart number, ADR-0032), split 1 / 2 / 2 across three offices. An average for 1 or 2 people is close to their own score, so a "totals only" view can still reveal a person.

**Options considered:**
1. Inflate the small groups in the data (breaks ADR-0032 and the "fix at the source" principle; invents people).
2. Leave aggregates as they are and rely on "totals only" (does not protect small groups).
3. Fix it in reporting: a minimum group size check before any summary is shown or saved.

**Decision:** Option 3 (NB04 Step 4.1.5).
- One setting, `MIN_GROUP_SIZE = 5`, and one function, `suppress_small_groups(df, group_col, count_col)`. Groups under the limit show "Suppressed (<5)" in every value column; the data is not changed.
- Applied to every summary of people in NB04: the cycles chart (Step 4.3.4), the new role and office breakdown (Step 4.3.5) and the KPI dashboard population check (Step 4.5.1a).
- Cycles chart: paths needing 3 or more cycles are grouped ("top band"), so no bar is a group of 1. Result 8 / 22 / 25; nothing suppressed. The raw count with a group of 1 is no longer printed.
- Role and office breakdown: by role, both groups shown (50 ID Leads, 5 Solution Architects). By role and office, the 3 Solution Architect groups (1, 2, 2) are suppressed.
- Not applied to Step 4.2 (counts courses, not people) or Step 4.4 (individual paths, protected by the own-path-only check, ADR-0052).

**Rationale (plain language):**
- This is standard practice in HR and L&D analytics: small cells are hidden at reporting time, never by changing the data.
- One setting makes the rule easy to tighten. Many HR teams use 10; at 10, Solution Architects would be hidden even at role level.
- Limitation: this is a simple threshold rule, not full statistical disclosure control (for example, it does not check whether hidden values can be worked out from totals across several tables). Described as GDPR-aligned, not compliant (ADR-0048).

**Status:** Accepted (extends ADR-0016, ADR-0048)

---

## Note: Thread 6 (NB04) close

NB04 (`NB04_visualization_demo.ipynb`) is complete: P14, P15 and the new P15a (path phasing) done. **Phase 1 is complete.** ADR-0050 to ADR-0055 added; total ADRs: 55.

NB04 is read only: no CSV changed and no schema change, so `04_schema_reference.md` is unchanged.

New artifacts in `reports/figures/`: `fig_4_2_curation_flow.png`, `fig_4_3_cycles_needed.png`, `fig_4_4_paths_idlead_vs_sa.png`, `fig_4_5_kpi_dashboard.png`.


Added before close (14 Oct 2026): minimum group size check (ADR-0055); role and office breakdown (Step 4.3.5); an "In real life" note that paths are revisited every year, since a 3-year plan does not suit fast-changing skills.

Next: Phase 2: GenAI upgrade, coming soon.
