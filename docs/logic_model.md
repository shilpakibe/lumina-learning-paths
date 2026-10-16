# Logic Model — Lumina Learning Group Personalized Learning Path Engine

A Logic Model is a program-evaluation framework widely used in L&D, nonprofits, and workforce development (a close cousin of Kirkpatrick's evaluation model) to trace a straight line from the resources a program draws on, through what it does, to the value it ultimately creates. It answers one core question: *if this program works, how do we know, and at what altitude do we look for evidence?*

It complements learning-design models such as ADDIE and COM-B: those answer "how do you design and deliver the learning," while the Logic Model answers "does this program actually create value, and how would you tell."

## The Generic Template

| Stage | Question it answers |
|---|---|
| **Inputs** | What resources exist before any work starts? |
| **Activities** | What actions/work get performed, using those resources? |
| **Outputs** | What direct, countable things get produced by those actions? |
| **Outcomes** | What individual/team-level change happens because someone received those outputs? |
| **Impact** | What organizational/business-level change happens once enough individual outcomes accumulate? |

## The Chain — Applied to This Project

| Stage | Content | Altitude |
|---|---|---|
| **Inputs** | Employee master data · Competency framework (competencies, KSBs, proficiency scale — pre-defined, typically by an external consultancy or internal COE, out of scope for this program) · Portfolio of learning objects | Resources that exist before any work starts |
| **Activities** | Design & administer the competency assessment · Tag learning objects to competencies/KSBs (matching engine) · **L&D review of uncertain matches (approve, switch, replace, or log a content gap)** · Assign courses to gaps and sequence them (recommendation engine) | The doing |
| **Outputs** | Tagged learning-object catalog (course ↔ KSB/competency mapping) · Computed employee competency gaps · **Reviewed course choices with an audit trail, and a content-gap list (e.g. KSB018)** · Personalized learning path recommendations · **Projected KPI movement (best case)** | Direct, countable products of the activities |
| **Outcomes** | Employee completes the assigned path → reassessed competency level rises → individual gap closes | Individual/team-level change |
| **Impact** | Enough individuals close their gaps → unit-level capability builds → business KPI is met | Organizational/business-level change, closes the loop back to the original strategic goals |

## Discipline Applied

The chain was built by testing each candidate item against a strict rule for its stage, rather than intuiting placement:

- **Input vs. Impact**: KPIs and business goals were initially miscategorized as Inputs. Corrected — a KPI is a *target you're measuring against*, not a *resource you draw on*. It belongs at the Impact end of the chain, not the Input end.
- **Activity vs. Output**: "Mapping competencies to employees" was initially treated as a separate activity from "administering the assessment." Corrected — in this program, the assessment *is* the mechanism by which competency requirements get applied to individual employees; they collapse into one activity.
- **Output vs. Outcome**: "The gap" and "the personalized recommendation" were initially miscategorized as Outcomes. Corrected — both are direct, computed/produced artifacts (an Output), not a change that has actually occurred. Producing a recommendation is not the same as anyone having benefited from it.
- **Outcome vs. Impact**: "KPI reduction" was initially conflated with individual-level change. Corrected — Outcome is what happens to *one employee* (they complete the path, their level rises, their gap closes). Impact is what happens only once that occurs *at scale* across the workforce, rolling up to the organizational KPI.
- **Projection vs. Impact (added Thread 5, NB03):** the KPI Impact Rollup (ADR-0046) shows how K2.1 and K2.2 *would* move if paths are completed. It is a computed estimate, so it sits in **Outputs**, not Impact. Real Impact needs measured results.

## Completion Is Not Competence (added Thread 5)

Finishing a path does not prove a gap is closed. Moving from Output to Outcome needs evidence from several sources:
- Reassessment in the next assessment cycle.
- Manager observation of the skill applied on the job.
- On-the-job practice and coaching (most learning happens through work, not courses).
- Work results (Kirkpatrick Levels 3 and 4).

This is why the NB03 KPI projections are labelled best case.

## Scope Boundary This Reveals

This project's build stops at **Outputs**. It generates tagged content, computed gaps, reviewed course choices, personalized recommendations, and best-case KPI projections — but does not simulate:

- **Outcomes**: no simulated course-completion or reassessment-after-completion data exists, so "did the employee's level actually rise" is not modeled.
- **Impact**: no measured aggregation of individual outcomes back into a moved KPI number exists. The NB03 rollup is a projection, not a measurement.

This is a deliberate scoping decision, not an oversight — simulating Outcomes/Impact honestly would require fabricating longitudinal completion and behavior-change data with no real grounding, which this project has consistently avoided doing elsewhere (see: real course catalog vs. synthetic, real performance data vs. out-of-scope). It is documented here explicitly so the boundary is legible to any reviewer. In short: *"the system is built through the Output stage — tagging, gap computation, L&D review, recommendation and a best-case KPI projection — Outcome and Impact are modeled conceptually via this Logic Model, but not simulated with fabricated behavioral data."*
