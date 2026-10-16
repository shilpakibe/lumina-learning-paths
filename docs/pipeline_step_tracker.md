# Pipeline Step Tracker — Conceptual (B) vs. Coding (P) Steps
### Lumina Learning Group (LLG) — AI-Personalized Learning Paths

Legend: **B** = Business/Conceptual step · **P** = Pipeline/Coding step

---

## Phase 0 — Foundation

| # | Step | Type | Discipline | Depends On | Status | Suggested Thread |
|---|---|---|---|---|---|---|
| B1 | Define org scenario (offices, revenue, structure) | Conceptual | — | — | ✅ Done | Thread 1 |
| B2 | Define strategic goals (5 total, 2 selected) | Conceptual | — | — | ✅ Done | Thread 1 |
| B3 | Define KPIs | Conceptual | — | B2 | ✅ Done | Thread 1 |
| B4 | Define competencies, KSBs, proficiency scale | Conceptual | — | — | ✅ Done | Thread 1 |
| B5 | Define required proficiency levels per role | Conceptual | — | B4 | ✅ Done | Thread 1 |

## Phase 1 — Personalized Learning Path Engine

| # | Step | Type | Discipline | Depends On | Status | Suggested Thread |
|---|---|---|---|---|---|---|
| B6 | Design assessment question bank (proficiency + COM-B) | Conceptual | — | B4 | ✅ Done | Thread 1.5 |
| B7 | Design GDPR access pattern | Conceptual | — | — | ✅ Done (built in NB03 Step 3.5, ADR-0048; reused for charts in NB04 Step 4.4, ADR-0052) | Thread 1.5 |
| B13 | Sketch conceptual architecture diagram (draft, technical + plain-language annotated versions) | Conceptual | — | B1-B7 | ✅ Done | Thread 1.5 |
| P1 | Finalize full table schema (all tables, relationships) | Coding-prep | Engineering | B1-B7, B13 | ✅ Done | Thread 2 |
| P2 | Generate master employee table (430, real org-chart-grounded) + PII split | Coding | DS | P1 | ✅ Done. **Thread 5 (ADR-0047):** `employee_id` re-keyed by joining order (1-1,400 pool with gaps) | NB01 thread (Thread 3) |
| P3 | Generate role/competency/KSB reference tables | Coding | DS | P1 | ✅ Done. **Thread 5 (ADR-0047):** `kpis` (with `kpi_type`, annual K2.2) and `kpi_competency_mapping` now generated in NB01 code | NB01 thread (Thread 3) |
| P4 | Generate assessment_questions + 2yr assessment_responses | Coding | DS | P1 | ✅ Done (regenerated Thread 5 after ID re-key) | NB01 thread (Thread 3) |
| P5 | Generate course catalog | Coding | DS | P1 | ✅ Done (Thread 4, ADR-0034): 154 rows (150 synthetic + 4 real arXiv), 30/30 KSBs covered at 3-5 courses each, 20 orphans. **Thread 5:** save protected by `REGENERATE_COURSES = False` (ADR-0047) | NB01 thread |
| P6 | Collect real-excerpt credibility sample | Coding | NLP | P5 | ✅ Done (CRS151-CRS154 abstracts pasted by user) | NB01 thread |
| P7 | Score responses → competency levels; compute gaps | Coding | DS | P4 | ✅ Done (NB02). Rebuilt Thread 5 after ID re-key: 1,650 rows, `gap_size` −2 to +3 | NB02 thread (Thread 4) |
| P8 | Auto-tag KSB layer via verb detection | Coding | NLP | P5 | ✅ Done (117 skill / 37 knowledge). Used as a fit check in paths (ADR-0045) | NB02 thread |
| P9 | Build TF-IDF matching engine | Coding | NLP | P5, P8 | ✅ Done (production) | NB02 thread |
| P9a | Build embedding-based matching engine (MiniLM), compare vs. TF-IDF | Coding | NLP | P5, P8 | ✅ Done (comparison; 17/30 top matches agree) | NB02 thread (ADR-0035) |
| P9b | Course-to-course duplicate detection | Coding | NLP/ML | P9a | ✅ Done (0.95 threshold, 27 courses in 12 clusters) | NB02 thread (ADR-0041) |
| P9c | Human curation review layer — `curation_status`, `is_recommended` | Coding | Engineering | P9, P9a | ✅ Done. NB02 rule (ADR-0042) + NB03 blind side-by-side L&D review (ADR-0043): 23 approved, 6 modified, 7 rejected; KSB018 content gap | NB02 + NB03 threads |
| P10 | Build course→competency classifier | Coding | ML | P5 | ✅ Done (benchmark only, non-discriminating, ADR-0039) | NB02 thread |
| P11 | Run LDA topic modeling on course catalog | Coding | NLP | P5 | ✅ Done (exploratory, k=12, ADR-0040) | NB02 thread |
| P12 | Build path/recommendation sequencing logic | Coding | ML | P7, P9c | ✅ **Done (NB03, ADR-0044, ADR-0045):** `learning_path_recommendations.csv`, 687 rows, 55 employees, 5-21 steps; biggest gap first, K-S-B order, shared course once, duplicate check, "coming soon" for content gaps | NB03 thread (Thread 5) |
| P12a | KPI Impact Rollup — trace approved recommendations back to `strategic_goals`/`kpis` | Coding | DS | P12 | ✅ **Done (NB03 Step 3.6, ADR-0046):** `outputs/kpi_impact_rollup.csv`. K2.2 actual 3.2 vs 6; projected 9.6-12.1. K2.1 now 9.1%; projected 70.9-100% vs 60% | NB03 thread (Thread 5) |
| P13 | Build per-employee-only access demo view | Coding | Engineering | P12 | ✅ **Done (NB03 Step 3.5, ADR-0048)** | NB03 thread (Thread 5) |
| P14 | Build aggregate-only KPI dashboard | Coding | DS | P7, P12a | ✅ **Done (NB04 Step 4.5, ADR-0051):** `fig_4_5_kpi_dashboard.png`, K2.2 and K2.1, actual vs projected vs target, best-case footnote. Minimum group size check on all people summaries (ADR-0055) | NB04 thread (Thread 6) |
| P15 | Visualize 2 contrasting employee paths | Coding | DS | P12 | ✅ **Done (NB04 Steps 4.2, 4.4, ADR-0051, ADR-0052):** curation flow (`fig_4_2_curation_flow.png`); EMP0476 ID Lead vs EMP0188 Solution Architect (`fig_4_4_paths_idlead_vs_sa.png`) with cycle dividers, KSB018 "coming soon", privacy gate | NB04 thread (Thread 6) |
| P15a | Path phasing over annual cycles | Coding | DS | P12 | ✅ **Done (NB04 Step 4.3, ADR-0050):** display only. Caps: 35 course hours (40 minus 5 mandatory) and 6 KSBs per cycle. 8 / 22 / 25 employees need 1 / 2 / 3 or more cycles (banded, ADR-0055); KSB cap set all 73 breaks (`fig_4_3_cycles_needed.png`). Role/office breakdown in Step 4.3.5 | NB04 thread (Thread 6) |

## Later phases

- Phase 2: GenAI upgrade, coming soon
- Phase 3: coming soon

## Documentation/Packaging

| # | Step | Type | Discipline | Depends On | Status | Suggested Thread |
|---|---|---|---|---|---|---|
| B11 | Write README (problem, architecture, results, roadmap) | Conceptual | — | all above | ✅ Done: embeds the architecture diagram and the 4 NB04 figures; plain-language layer only (ADR-0054) | GitHub thread |
| B12 | Write ADRs for all thread decisions | Conceptual | — | ongoing | 🔄 Ongoing (55 ADRs as of Thread 6 close) | Every thread |
| B14 | Finalize polished architecture diagram | Conceptual | — | Phase 1 built | ✅ Done: `reports/figures/fig_0_architecture.png` | GitHub thread |
| P28 | GitHub packaging (LICENSE, requirements.txt, repo setup) | Coding | Engineering | B11 | 🔄 In progress: requirements.txt (incl. sentence-transformers, networkx, gensim); split licence (code: PolyForm Noncommercial 1.0.0; data, docs, figures: CC BY-NC 4.0, ADR-0049) | GitHub thread |

---

## Notes

- `03_pipeline_steps.xlsx` (Thread 1's original list) is superseded by this tracker; kept as historical reference only.
- All Phase 1 input data (P2–P6) is generated **once, upfront, in NB01**. Fixes go into NB01 code, not hand-edited CSVs (ADR-0047).
- **Thread 4** pulled forward P9a/P9b/P9c/P12a (ADR-0033 to ADR-0036).
- **Thread 5 (NB03)** completed P12, P12a and P13; resolved all four NB02 open items (ADR-0043 to ADR-0046); fixed KPIs and employee IDs at source and added re-run guards (ADR-0047).
- **Thread 6 (NB04)** completed P14, P15 and added P15a (path phasing). NB04 is read only: no CSV changed, no schema change. **Phase 1 is complete.** ADR-0050 to ADR-0055 added (ADR-0055: minimum group size).
- Next: Phase 2: GenAI upgrade, coming soon.
