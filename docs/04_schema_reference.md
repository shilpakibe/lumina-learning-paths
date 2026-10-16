# Schema Reference — Lumina Learning Group (LLG) AI-Personalized Learning Paths

Field-level reference for all 19 tables (18 original + `kpi_competency_mapping`, added Thread 4), finalized in Thread 2, with field additions from Thread 3 (NB01 build), Thread 4 (NB01 re-run and NB02 build) and Thread 5 (NB03 build) — see ADR-0025 through ADR-0048 in `decisions.md`.

Table counts: 16 input tables (generated in NB01, incl. `kpi_competency_mapping`) + 3 derived tables (`employee_ksb_gaps`, `course_ksb_mapping`, `learning_path_recommendations`) = 19. All 19 now exist.

---

## Schema Conventions

These six conventions apply across all tables unless explicitly flagged as an exception.

| # | Convention | Rule |
|---|---|---|
| 1 | **Primary key style** | `<table_singular>_id`, string, prefix code + zero-padded number (e.g. `EMP0001`, `CRS001`, `KSB001`) |
| 2 | **Foreign key naming** | FK columns always match the parent table's PK column name exactly — no renaming |
| 3 | **Timestamps** | Skipped by default (no generic `created_at`/`updated_at`). Real date fields kept only where genuinely used downstream (e.g. `assessment_responses.assessment_date`) |
| 4 | **Phase tagging** | Fields are tagged `P1` (used in Phase 1) or `P1-reserved` (reserved, empty in Phase 1) |
| 5 | **Reserved columns** | Reserved columns are typed and NULL-able now, not added later |
| 6 | **Categorical fields** | Documented enum — full allowed-value list written in this doc, not DB-enforced (this is pandas/CSV, not a live RDBMS) |

**Two exceptions to convention #1** (see ADR-0026):
- `proficiency_scale.proficiency_level` — plain integer (1–4), not string-prefixed, since it's used in direct arithmetic (`required_level − actual_level = gap_size`)
- `role_competency_requirements` — no surrogate PK; natural key is the `(role_id, competency_id)` pair, since it's a pure junction table nothing else references by row ID

`kpi_competency_mapping` (added Thread 4, ADR-0037) follows the same no-surrogate-PK pattern as `role_competency_requirements` — natural key `(kpi_id, competency_id)`.

**File encoding:** all CSVs are UTF-8. A CSV edited by hand (e.g. pasting abstracts into `courses.csv`, or the curation review sheet) must be saved as CSV UTF-8, otherwise `pandas.read_csv` raises `UnicodeDecodeError`.

**Source of truth (ADR-0047):** every generated table is defined in notebook code. Fixes are made in the generating code, never by hand in the CSV, since a re-run overwrites hand edits. `courses.csv` is the one enriched file; its NB01 save is protected by `REGENERATE_COURSES = False`.

---

## Cluster 1 — Org / Reference Tables

### 1. `orgs`
Single-row table.

| Field | Type | Notes |
|---|---|---|
| `org_id` | string PK | `ORG001` |
| `org_name` | text | |
| `founded_year` | int | |
| `revenue_usd` | int | |
| `headcount_total` | int | |

### 2. `offices`

| Field | Type | Notes |
|---|---|---|
| `office_id` | string PK | `OFF001` |
| `org_id` | FK → `orgs.org_id` | |
| `office_name` | text | |
| `office_role_description` | text | |

### 3. `job_families`

| Field | Type | Notes |
|---|---|---|
| `job_family_id` | string PK | `JF01` |
| `job_family_name` | enum text | `Instructional Design` / `Content/Solution Architecture` |

### 4. `roles`

| Field | Type | Notes |
|---|---|---|
| `role_id` | string PK | `ROLE01` |
| `job_family_id` | FK → `job_families.job_family_id` | |
| `role_name` | text | |
| `is_phase1_focus_role` | boolean | flags the 2 spotlighted roles (Instructional Design Lead, Solution Architect) |

### 5. `strategic_goals`

| Field | Type | Notes |
|---|---|---|
| `goal_id` | string PK | `G1`, `G2` |
| `goal_text` | text | |
| `status` | enum text | `active` / `parked` |

### 6. `kpis`
Generated in NB01 C1.6 code (ADR-0047), including `kpi_type` and the annual K2.2 wording.

| Field | Type | Notes |
|---|---|---|
| `kpi_id` | string PK | `K1.1` |
| `goal_id` | FK → `strategic_goals.goal_id` | |
| `kpi_text` | text | K2.2 reworded Thread 4 (ADR-0037) — see below |
| `target_value` | text | mixed units (%, ratio) — kept as text. K2.2 changed from `1.5 KSBs/quarter` to `6 KSBs/year` (ADR-0037) |
| `target_date` | text | e.g. "FY27" — not a real date |
| `kpi_type` | enum text | **Added Thread 4 (ADR-0037).** `qualitative_program_level` (K1.1, K1.2 — no individual competency signal, tracked externally in real orgs) / `computed_competency_driven` (K2.1 — traces to specific weighted competencies via `kpi_competency_mapping`) / `computed_aggregate` (K2.2 — spans whichever competencies apply per employee by construction; bypasses `kpi_competency_mapping` entirely) |

**K2.2 wording (Thread 4, ADR-0037):** originally "Average KSB gap-closure rate per employee per quarter" / `1.5 KSBs/quarter`. `assessment_responses` only has two cycles per *year* (`prior_year`/`current_year`, ADR-0018), so a literal quarterly rate isn't computable without fabricating a quarterly assessment cadence. Reworded to "Average KSB gap-closure rate per employee per assessment cycle (annual)" / `6 KSBs/year` — same ambition, restated at the cadence the data supports.

### 6a. `kpi_competency_mapping` — *exception to convention #1, new Thread 4 (ADR-0037)*

Many-to-many bridge between `kpis` and `competencies`, populated **only for K2.1** — the one KPI that is genuinely competency-driven. K1.1/K1.2 (`qualitative_program_level`) and K2.2 (`computed_aggregate`) are deliberately excluded; see `kpi_type` above and ADR-0037 for why. Generated in NB01 C1.6a code since Thread 5 (ADR-0047); previously hand-made.

| Field | Type | Notes |
|---|---|---|
| `kpi_id` | FK → `kpis.kpi_id` | part of natural key |
| `competency_id` | FK → `competencies.competency_id` | part of natural key |
| `contribution_weight` | enum text | `primary` / `secondary` — not a numeric score; mirrors how competencies genuinely contribute unevenly to a KPI rather than flattening them to equal weight |

No surrogate PK — natural key is `(kpi_id, competency_id)`, same convention as `role_competency_requirements` (ADR-0026). Currently 5 rows, all `kpi_id = K2.1`: `COMP01`/`COMP02`/`COMP07` primary, `COMP03`/`COMP04` secondary.

### 7. `competencies`

| Field | Type | Notes |
|---|---|---|
| `competency_id` | string PK | `COMP01`–`COMP10` |
| `competency_name` | text | |

### 8. `ksbs`

| Field | Type | Notes |
|---|---|---|
| `ksb_id` | string PK | `KSB001`–`KSB030` |
| `competency_id` | FK → `competencies.competency_id` | |
| `ksb_text` | text | |
| `ksb_type` | enum text | `knowledge` / `skill` / `behavior`. Also drives K-S-B path ordering in NB03 (ADR-0045). |

### 9. `proficiency_scale` — *exception to convention #1*

| Field | Type | Notes |
|---|---|---|
| `proficiency_level` | int PK (1–4) | not string-prefixed — used in direct gap arithmetic |
| `level_label` | text | e.g. "Practitioner" |
| `level_description` | text | |

### 10. `role_competency_requirements` — *exception to convention #1*

| Field | Type | Notes |
|---|---|---|
| `role_id` | FK → `roles.role_id` | part of natural key |
| `competency_id` | FK → `competencies.competency_id` | part of natural key |
| `required_level` | FK → `proficiency_scale.proficiency_level` | |

No surrogate PK — natural key is `(role_id, competency_id)`. Populated only for the 2 Phase 1 focus roles (Instructional Design Lead, Solution Architect) — 20 rows (2 roles × 10 competencies).

---

## Cluster 2 — People

### 11. `employees`
430 rows — real-org-chart-grounded headcount for the 2 in-scope job families (JF01, JF02) across all 4 roles, not an arbitrary org-wide estimate. See ADR-0032.

| Field | Type | Notes |
|---|---|---|
| `employee_id` | string PK | **Re-keyed Thread 5 (ADR-0047).** Assigned in joining order (hire year = 2026 − `tenure_years`), drawn from a 1–1,400 pool with gaps standing in for other departments and leavers. Non-contiguous, observed EMP0003–EMP1399. Low IDs = longest tenure; roles are interleaved across the range. |
| `office_id` | FK → `offices.office_id` | Content Designer/ID Lead/ID Manager (425) → Pune (`OFF001`); Solution Architect (5) → 1 Pune, 2 Austin, 2 Manchester |
| `role_id` | FK → `roles.role_id` | job_family reachable via `roles`, not duplicated |
| `vertical` | enum text | **Added Thread 3 (ADR-0029).** One of the 5 client verticals (`Pharma` / `Telecom` / `Banking/Finance` / `ERP/Software` / `Call Center BPO`), matching the org chart's even per-vertical headcount split for each role. Single static value — no vertical-transition history modeled. |
| `tenure_years` | int | Capped at 15 (org founded 2011). Sampled by career grade — pyramid-shaped (CG50 skews low, CG80 skews high), not flat/uniform. |
| `certification_count` | int | Uniform baseline (vertical-onboarding + role-tool certs, per ADR-0029) + variable 0–5 (DS/ML/GenAI upskilling progress, K2.1) — same baseline logic for all 4 roles, since Solution Architects carry their vertical certs forward from the CG70 track. |

**Role/office/headcount detail (from `20260907OPModel.xlsx`, real org chart):**

| role_id | role_name | career_grade | total | office split |
|---|---|---|---|---|
| ROLE03 | Content Designer | CG50 | 350 | all Pune |
| ROLE01 | Instructional Design Lead | CG60 | 50 | all Pune |
| ROLE02 | Instructional Design Manager | CG70 | 25 | all Pune |
| ROLE04 | Solution Architect | CG80 | 5 | 1 Pune, 2 Austin, 2 Manchester |

### 12. `employees_pii`
1:1 with `employees`, 430 rows. No FK relationship into any engine table — joined only at display layer (ADR-0023). Used in NB03 Step 3.5 access demo only (ADR-0048).

| Field | Type | Notes |
|---|---|---|
| `employee_id` | FK → `employees.employee_id` (also acts as PK) | |
| `first_name` | text | diverse pool (gender + cultural representation) |
| `last_name` | text | synthetic 3-letter combo, e.g. "OPQ" |
| `full_name` | text | derived: `first_name + " " + last_name` |
| `email` | text | synthetic domain `@lumina-learning.com`, not a real address |

---

## Cluster 3 — Assessment

### 13. `assessment_questions`
Lookup table, 16 rows.

| Field | Type | Notes |
|---|---|---|
| `question_id` | string PK | `Q001`–`Q016` |
| `question_text` | text | |
| `question_type` | enum text | `proficiency` (10 rows, one per competency) / COM-B questions: `comb_capability` / `comb_opportunity` / `comb_motivation` (2 each, 6 total). COM-B = Capability, Opportunity, Motivation, a behavioural-science model of why a skill is or is not applied. Collected and held for later use. |
| `linked_competency_id` | FK → `competencies.competency_id` | |
| `response_scale` | text | `proficiency` rows → validated against `proficiency_scale` (1–4); `comb_*` (COM-B) rows → 1–5 Likert, described here as free text, no dedicated lookup table |

### 14. `assessment_responses`
Long-format fact table. 1,760 rows — scoped to the 55 employees in the 2 Phase 1 focus roles (Instructional Design Lead, Solution Architect) × 16 questions × 2 cycles. Regenerated in Thread 5 after the employee ID re-key (ADR-0047); same structure and counts.

| Field | Type | Notes |
|---|---|---|
| `response_id` | string PK | `RESP000001` — surrogate, natural key is 3 columns wide |
| `employee_id` | FK → `employees.employee_id` | scoped to focus-role employees only |
| `question_id` | FK → `assessment_questions.question_id` | |
| `response_value` | int | scale depends on `question_type`; proficiency values sampled with realistic variance around/below each employee's role-required level (real gaps, not artificially floored), with a gentle upward drift from `prior_year` to `current_year` |
| `assessment_date` | date | the one meaningful timestamp kept |
| `assessment_cycle` | enum text | `current_year` / `prior_year` — explicit, not inferred from date |

---

## Cluster 4 — Courses

### 15. `courses`
154 rows — 150 procedurally-generated synthetic rows (a `COURSE_SPEC`-style generator, ADR-0034) + 4 real arXiv-abstract credibility sample rows (`CRS151`–`CRS154`, abstracts and citations completed by the user). All 30 KSBs are covered by 3–5 courses each; 20 courses are orphan/generic (no intended KSB). NB01 save protected by `REGENERATE_COURSES = False` (ADR-0047).

| Field | Type | Phase | Notes |
|---|---|---|---|
| `course_id` | string PK | P1 | `CRS001`–`CRS154` |
| `course_title` | text | P1 | Titles come from templates, so two different courses can share a title (e.g. CRS083, CRS089). Flagged in curation review (ADR-0043). |
| `description_raw` | text | P1 | full text — feeds embeddings directly, no re-collection needed |
| `course_source` | enum text | P1 | **Added Thread 3 (ADR-0031).** `Lumina_LMS` (fictional internal LMS) / `KonectIn` (fictional stand-in for LinkedIn Learning) / `Ksera` (fictional stand-in for Coursera) / `Hiword` (fictional stand-in for Harvard-style content) / `arXiv` (real-excerpt rows). Mix: Lumina_LMS 91 / KonectIn 23 / Ksera 20 / Hiword 16 / arXiv 4. Generic/orphan and redundant courses skew toward the external platforms, simulating a realistic multi-source ingestion layer. Shown to the curation reviewer (ADR-0043). |
| `is_real_excerpt` | boolean | P1 | flags the arXiv-abstract rows |
| `credibility_sample_label` | text | P1 | citation for real-excerpt rows only, e.g. "arXiv:2508.19569 — [title], abstract, [license]"; NULL for synthetic rows |
| `duration_minutes` | int | P1 | NULL for real-excerpt rows. Summed per path for "hours of learning" in the NB03 access demo. |
| `format` | enum text | P1 | `video` / `course` / `job-aid` / `checklist` |
| `target_ksb_id` | nullable FK → `ksbs.ksb_id` | P1 | **Added Thread 4 (ADR-0038).** The KSB the generator wrote the course to teach. Answer key for accuracy checks only (NB02 2.7, 2.7.6). NULL for 24 rows: 20 orphans + 4 arXiv. Not a production input; hidden from the NB03 curation reviewer (blind review). |
| `target_competency_id` | nullable FK → `competencies.competency_id` | P1 | **Added Thread 4 (ADR-0038).** Competency of `target_ksb_id`. Used as the label for the classifier and LDA cross-tab. Same 24 NULLs. |
| `ksb_layer` | enum text | P1 | `knowledge` / `skill` — via verb-detection; populated in NB02 (P8): 117 skill / 37 knowledge. **Use decided Thread 5 (ADR-0045):** fit check only; mismatches with the KSB's `ksb_type` are noted in the path rationale. Does not reorder or drop courses. |
| `likely_duplicate_of` | nullable FK → `courses.course_id` | P1, populated NB02 | **Added Thread 4 (ADR-0035, ADR-0041).** Set when the course is a near-duplicate (course-to-course cosine ≥ 0.95) of the cluster's representative course; NULL otherwise. Detection only; nothing is removed or merged. |
| `duplicate_cluster_id` | nullable string | P1, populated NB02 | **Added Thread 4 (ADR-0041).** `DUPCLUST###`, from connected components over flagged pairs. 27 courses in 12 clusters; NULL if not in a cluster. Used in NB03's per-path duplicate check (0 swaps needed, ADR-0045). |
| `core_learning_or_behavioral_enabler` | enum text | P1-reserved | Reserved. Empty in Phase 1. |
| `comb_blueprint` | text | P1-reserved | Reserved. Empty in Phase 1. |

*Not part of the schema:* NB02 builds a helper column `course_text_feature` (title + description combined) for vectorizing. It is a working column only and should not be saved to `courses.csv` as a schema field.

---

## Cluster 5 — Derived Tables

### 16. `employee_ksb_gaps`
Scoped to `proficiency`-type responses only (ADR-0023); COM-B responses are not used here. Built in NB02 (P7), not NB01. 1,650 rows. Rebuilt in Thread 5 after the employee ID re-key (ADR-0047).

| Field | Type | Notes |
|---|---|---|
| `gap_id` | string PK | surrogate |
| `employee_id` | FK → `employees.employee_id` | |
| `ksb_id` | FK → `ksbs.ksb_id` | |
| `required_level` | FK → `proficiency_scale.proficiency_level` | pulled via employee's role → `role_competency_requirements` |
| `actual_level` | FK → `proficiency_scale.proficiency_level` | computed from `assessment_responses` (current_year cycle) |
| `gap_size` | int | `required_level − actual_level`, precomputed. **Observed range −2 to +3** (Thread 5 re-draw). Positive = below requirement (a gap); zero = meets; negative = above requirement (no learning need). NB03 filters to `gap_size > 0`: 687 open gaps (540 × 1, 144 × 2, 3 × 3). |

*Note: assessment measures at competency level, not KSB level — all 3 KSBs under a competency inherit the same `required_level`/`actual_level`/`gap_size`.*

*Note (ADR-0037, ADR-0046): K2.2's actual gap-closure rate is computed from both assessment cycles (a competency gap that moved from positive to zero or below, × 3 KSBs), without routing through `kpi_competency_mapping`, since the KPI is competency-agnostic by construction.*

### 17. `course_ksb_mapping`
Classifier and LDA outputs remain comparison/exploratory only, saved as notebook artifacts (`outputs/`), not persisted here (ADR-0023, ADR-0039, ADR-0040). Both `tfidf` and `embedding` match rows ARE persisted (ADR-0035): one row per course-KSB pair per method. Built in NB02 (P9/P9a), not NB01. **9,240 rows** = 154 courses × 30 KSBs = 4,620 pairs per method × 2 methods.

| Field | Type | Phase | Notes |
|---|---|---|---|
| `mapping_id` | string PK | P1 | surrogate |
| `course_id` | FK → `courses.course_id` | P1 | |
| `ksb_id` | FK → `ksbs.ksb_id` | P1 | |
| `match_score` | float | P1 | TF-IDF cosine similarity / embedding cosine similarity (comparison) |
| `match_method` | enum text | P1 | `tfidf` (production) / `embedding` (comparison, ADR-0035) |
| `match_explanation` | text | P1 | top shared keywords (`tfidf`); generic semantic-similarity note (`embedding`) |
| `is_production` | boolean | P1 | **Added Thread 4 (ADR-0035).** True for the rows the Path Engine consumes (`tfidf`). |
| `is_recommended` | boolean | P1, populated NB02 | **Added Thread 4 (ADR-0036).** True only for the top-scoring course per KSB, flagged **per method** (30 true for `tfidf`, 30 true for `embedding`). Not changed by curation. |
| `curation_status` | enum text | P1, updated NB02/NB03 | **Added Thread 4 (ADR-0036, ADR-0042); resolved Thread 5 (ADR-0043).** `algo_suggested` / `ld_reviewed` / `ld_approved` / `ld_modified` / `ld_rejected`. After the NB03 L&D review, TF-IDF rows: 23 `ld_approved`, 6 `ld_modified`, 7 `ld_rejected`, 0 `ld_reviewed`, rest `algo_suggested`. Embedding rows stay `algo_suggested`. Exactly one `ld_approved`/`ld_modified` row per KSB for 29 of 30 KSBs; KSB018 has none (content gap, ADR-0044). Only `ld_approved`/`ld_modified` are eligible input to `learning_path_recommendations`. |

### 18. `learning_path_recommendations`
Built in NB03 (P12, ADR-0044, ADR-0045). **687 rows** = one row per open gap (`gap_size > 0`) for all 55 focus-role employees.

| Field | Type | Phase | Notes |
|---|---|---|---|
| `recommendation_id` | string PK | P1 | surrogate, `REC00001`; ordered by employee, then step |
| `employee_id` | FK → `employees.employee_id` | P1 | |
| `course_id` | nullable FK → `courses.course_id` | P1 | **Nullable for content-gap rows only** (19 rows, all KSB018, ADR-0044) |
| `gap_id` | FK → `employee_ksb_gaps.gap_id` | P1 | which specific gap this row addresses; unique per row |
| `sequence_order` | int | P1 | step in the employee's path (1 = first). Rows sharing a course share one step (ADR-0045). Paths run 5–21 steps (mean 11.6). |
| `recommendation_method` | enum text | P1 | `rule_based` |
| `rationale` | text | P1 | rule output: gap size, competency, K/S/B type, "Also covers", fit-check note; content-gap message for KSB018 |

---

## Notebook artifacts (not tables)

Saved to `outputs/`, not persisted to any table (ADR-0023):
- `lda_topic_words.csv` — top words per LDA topic (k=12, ADR-0040) [NB02]
- `lda_topic_vs_competency.csv` — topic × `target_competency_id` cross-tab [NB02]
- `curation_review_sheet.csv` — side-by-side review export, 19 rows; audit copy, protected from re-run overwrite (ADR-0043, ADR-0047) [NB03]
- `curation_review_sheet_completed.csv` — the L&D reviewer's decisions (`decision`, `replacement_course_id`, `reviewer_note`); hand-edited, CSV UTF-8, read-only to code (ADR-0043) [NB03]
- `kpi_impact_rollup.csv` — aggregate K2.1/K2.2 actual and projected values, 6 rows, no individual data (ADR-0046) [NB03]

---

## Finalized KSB List (30 total)

3 per competency (Knowledge / Skill / Behavior), finalized from the Thread 1 draft with no content changes.

| ksb_id | competency_id | ksb_type | ksb_text |
|---|---|---|---|
| KSB001 | COMP01 | knowledge | Understands capabilities/limitations of GenAI tools for content drafting |
| KSB002 | COMP01 | skill | Can prompt-engineer GenAI tools to produce first-draft learning content |
| KSB003 | COMP01 | behavior | Iteratively reviews and refines AI-drafted content rather than accepting output as-is |
| KSB004 | COMP02 | knowledge | Understands key learning metrics (completion rate, assessment scores, time-to-competency) |
| KSB005 | COMP02 | skill | Can build/read dashboards to track learning program effectiveness |
| KSB006 | COMP02 | behavior | Uses data to justify design decisions, not just intuition |
| KSB007 | COMP03 | knowledge | Understands basic NLP concepts (keyword extraction, embeddings, similarity matching) |
| KSB008 | COMP03 | skill | Can apply keyword/embedding-based tagging to classify content against a taxonomy |
| KSB009 | COMP03 | behavior | Validates NLP-driven tags/matches against subject-matter judgment before publishing |
| KSB010 | COMP04 | knowledge | Understands principles of adaptive learning path design |
| KSB011 | COMP04 | skill | Can configure/design a learner profile → path mapping logic |
| KSB012 | COMP04 | behavior | Designs for diverse learner starting points, not a single "average" learner |
| KSB013 | COMP05 | knowledge | Knows ADDIE / competency-based design models |
| KSB014 | COMP05 | skill | Can write measurable learning objectives |
| KSB015 | COMP05 | behavior | Applies design consistently across content types |
| KSB016 | COMP06 | knowledge | Understands client's business context and compliance needs |
| KSB017 | COMP06 | skill | Can translate vague client asks into a structured learning solution brief |
| KSB018 | COMP06 | behavior | Proactively manages client expectations on AI-generated content accuracy |
| KSB019 | COMP07 | knowledge | Knows landscape of GenAI authoring/content tools |
| KSB020 | COMP07 | skill | Can operate at least one GenAI content tool end-to-end in a real workflow |
| KSB021 | COMP07 | behavior | Stays current with new tool releases relevant to L&D |
| KSB022 | COMP08 | knowledge | Understands system components of a learning tech stack (LMS, content repo, analytics) |
| KSB023 | COMP08 | skill | Can design a data flow diagram for a personalized learning system |
| KSB024 | COMP08 | behavior | Designs for scalability/reusability, not one-off solutions |
| KSB025 | COMP09 | knowledge | Understands KSB frameworks and taxonomy design principles |
| KSB026 | COMP09 | skill | Can decompose a competency into measurable KSBs |
| KSB027 | COMP09 | behavior | Keeps taxonomy consistent and non-redundant across roles |
| KSB028 | COMP10 | knowledge | Understands common AI content risks (hallucination, bias, compliance gaps) |
| KSB029 | COMP10 | skill | Can run a structured QA review checklist on AI-generated content |
| KSB030 | COMP10 | behavior | Flags/escalates content that fails QA rather than letting it pass silently |

---

## KSB Derivation Methodology

KSBs in this project were derived and structured using Bloom's Taxonomy (Knowledge) and Krathwohl's Affective Domain (Behavior). In practice, DACUM-style SME elicitation is a well-established way to derive KSBs from real job/task analysis; where that isn't available, L&D practitioners can use Bloom's and Krathwohl's taxonomies to derive and structure them instead.

---

## Table Relationships (join map)

```
orgs → offices → employees
job_families → roles → employees
roles + competencies + proficiency_scale → role_competency_requirements
competencies → ksbs
competencies → assessment_questions
strategic_goals → kpis
kpis (K2.1 only) + competencies → kpi_competency_mapping   [Thread 4, ADR-0037]

employees ↔ employees_pii (1:1, display-layer join only)
employees + assessment_questions → assessment_responses (scoped to focus-role employees)

assessment_responses + role_competency_requirements → employee_ksb_gaps
courses + ksbs → course_ksb_mapping
courses → courses (likely_duplicate_of, duplicate_cluster_id)   [Thread 4, ADR-0041]
courses.target_ksb_id → ksbs, courses.target_competency_id → competencies   [answer key, ADR-0038]
course_ksb_mapping (ld_approved/ld_modified only) → employee_ksb_gaps → learning_path_recommendations
learning_path_recommendations + kpis → KPI Impact Rollup (no new table — outputs/kpi_impact_rollup.csv)
```

**Engine data flow (updated Thread 5 — see ADR-0043 to ADR-0046):**
`assessment_responses` → **Diagnostic/Gap Engine** → `employee_ksb_gaps`
`courses` + `ksbs` → **Matching Engine** (TF-IDF production + embedding comparison, ADR-0035; course-to-course clustering → `likely_duplicate_of`, `duplicate_cluster_id`) → `course_ksb_mapping`
`course_ksb_mapping` → **L&D Curation Review** (NB02 rule, ADR-0042; NB03 blind side-by-side review, ADR-0043) → only `ld_approved`/`ld_modified` rows proceed
`employee_ksb_gaps` (gap_size > 0) + reviewed `course_ksb_mapping` → **Recommendation/Path Engine** (rule-based sequencing, ADR-0045; content gaps as "coming soon", ADR-0044) → `learning_path_recommendations`
`learning_path_recommendations` → **KPI Impact Rollup** (NB03, ADR-0046) → K2.1 via `kpi_competency_mapping` primary competencies; K2.2 from both assessment cycles; K1.1/K1.2 out of scope (`qualitative_program_level`)

---

## Scope note: multi-category learning paths (not built)

Real-world L&D also carries parallel *mandatory* tracks distinct from the AI-upskilling competencies this project builds — vertical domain onboarding (e.g. Telecom Basics for the Telecom vertical), tool proficiency (e.g. Articulate Storyline, HTML), and general business principles for all staff. To extend this system to cover them: add a `training_category` dimension to the competency/KSB schema (`ai_upskilling` / `vertical_onboarding` / `tool_proficiency` / `general_business`), each with its own requirement rule (org-wide mandatory, vertical-conditional, role-conditional) rather than a computed proficiency gap. Matching/recommendation logic would stay largely reusable, but requirement computation would shift from gap-based to rule-based eligibility for these categories, since domain onboarding is binary done/not-done, not a 1–4 proficiency scale. See ADR-0030.

---

*Document generated in Thread 2 (Full Schema Formalization); updated in Thread 3 (NB01 build) per ADR-0029 through ADR-0032; updated in Thread 4 per ADR-0033 through ADR-0037 (procedural course catalog, embeddings/dedup, human curation layer, KPI Impact Rollup, KPI-competency traceability); updated at the close of Thread 4 (NB02) per ADR-0038 through ADR-0042; updated at the close of Thread 5 (NB03) per ADR-0043 through ADR-0048 (L&D review, content gaps, path sequencing, KPI rollup, source-of-truth fixes and employee ID re-key, privacy demo).*
