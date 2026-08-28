# **Data Quality Pitfalls: From Recurring Failure Modes to a Cross-Industry Prevention Framework**

**Krishna Murthy Kodiganti**
*Senior Lead Software Engineer, Capital One*

*The views expressed in this paper are the author's own and do not represent the views of Capital One.*

*[Working title — revisit once Section 3's framework is fleshed out.]*

---

# **Abstract**

*[Draft — needs citations before this leaves scaffold stage. Structure to follow: cost/scale hook, thesis statement, taxonomy preview, framework preview, outcome preview. Two paragraphs: first grounds the problem in cross-industry cost figures, second states the proposed framework and reported outcomes.]*

Organizations across every industry treat data-quality failures as isolated incidents: a bad report here, a failed compliance filing there, a machine learning model that quietly degrades in production. Poor data quality costs the U.S. economy an estimated $3 trillion annually (Redman, 2016) [CITATION NEEDED — verify against research file Section 0], and the pattern repeats at the level of individual organizations: [CITATION NEEDED — per-enterprise annual cost figure].

This paper argues [revise per tone convention — state directly instead] that these failures are not independent incidents but symptoms of the same missing infrastructure, recurring across six failure-mode categories: structural, semantic, identity/duplication, statistical (including ML-specific drift and bias), temporal, and governance. It proposes a three-part prevention framework — **data contracts at the boundary**, **continuous observability and lineage**, and **named ownership and accountability**. Organizations that adopt this kind of framework report [CITATION NEEDED — outcome figures: incident reduction, MTTR improvement, etc.].

---

# **1. Introduction**

## **Background**

*[Draft narrative, no citations yet.]*

Every organization that collects, stores, or acts on data is implicitly betting that the data reflects reality closely enough to make a good decision from it. That bet fails constantly, and usually silently: a schema changes upstream and nobody downstream is told; two teams use the same column name to mean two different things; a duplicate customer record splits one person's history into two profiles; a training set drifts away from the population a model now serves in production; a nightly job fails partway through and nobody notices for three weeks. None of these are exotic. They are the ordinary condition of production data, and they occur at a comparable rate regardless of industry, team maturity, or the sophistication of the systems built on top of the data.

## **Problem Statement**

Organizations treat these failures as one-off incidents to be triaged and forgotten, rather than as symptomatic of a missing layer of infrastructure. This paper's core question: **how should any organization, in any industry, systematically prevent — rather than repeatedly firefight — the recurring failure modes that make data untrustworthy at the point of use?**

---

# **2. A Taxonomy of Data Quality Pitfalls**

*[Outline stage — each subsection needs a citation before it leaves scaffold. Ordered roughly from "closest to ingestion" to "closest to organizational process."]*

## **2.1 Structural Pitfalls**

Schema drift (a producer changes a field's type, name, or nullability without notice), encoding/format inconsistency (dates, currencies, units mixed across sources), and silent truncation or type coercion at ingestion. `[CITATION NEEDED]`

## **2.2 Semantic Pitfalls**

The same field name means different things in different systems or teams (five definitions of "active customer" across five departments), unit mismatches that pass type validation but not meaning ("amount" in cents in one system, dollars in another), and undocumented business logic embedded in ETL rather than in a shared definition. `[CITATION NEEDED]`

## **2.3 Identity & Duplication Pitfalls**

Fragmented or duplicate records for the same real-world entity: the same person or organization exists as separate, disconnected records because names, addresses, or identifiers were captured inconsistently across systems. Reported duplicate-record rates commonly run 10–30% in financial services and 10–22% in healthcare. `[CITATION NEEDED — source independently]`

## **2.4 Statistical & Distributional Pitfalls (including ML-specific)**

Training/serving skew, label noise, sampling bias, distribution drift after deployment, and feedback loops where a model's own outputs pollute the data used to retrain it. Candidate anchor: Sambasivan et al., "Data Cascades in High-Stakes AI" (CHI 2021) — peer-reviewed, argues data quality problems compound invisibly through ML pipelines. `[CITATION NEEDED — verify this paper's exact findings before citing]`

## **2.5 Temporal Pitfalls**

Staleness (data consumed after it has stopped reflecting reality), latency (correct data arriving too late to act on), out-of-order events, and silent backfills that change historical values after downstream consumers have already acted on the old ones. `[CITATION NEEDED]`

## **2.6 Governance & Accountability Pitfalls**

No named data owner per dataset, no data-quality SLAs, no lineage showing what depends on what, tribal knowledge instead of documentation, and data quality treated as a one-time cleanup project rather than a continuous discipline. `[CITATION NEEDED — DAMA-DMBOK or similar standards-body framing]`

| Pitfall Category | Where It Originates | Typical Symptom |
| ----- | ----- | ----- |
| Structural | Ingestion / schema boundary | Pipeline breaks, silent nulls |
| Semantic | Cross-team / cross-system definitions | Metrics disagree across dashboards |
| Identity & Duplication | Entity matching across sources | Duplicate/fragmented customer records |
| Statistical & Distributional | Model training / production drift | Model accuracy decays silently |
| Temporal | Data freshness / event ordering | Decisions made on stale data |
| Governance & Accountability | Organizational process | No one owns fixing it |

---

# **3. The Proposed Framework: Data Quality as Infrastructure**

*[Outline stage — this is the paper's proposed reference architecture. Needs outcome evidence before it leaves scaffold.]*

## **3.1 Overview**

Three components, addressing the six pitfall categories above as a system rather than case-by-case:

1. **Data contracts at the boundary** — a versioned, enforced interface between a data producer and consumer (schema + semantic meaning + SLA), checked in CI rather than discovered in production. Addresses structural and semantic pitfalls directly.
2. **Continuous observability and lineage** — automated monitoring for freshness, volume, schema, and distributional anomalies, plus traceability of what depends on what. Addresses temporal and statistical/distributional pitfalls, and makes governance gaps visible.
3. **Named ownership and accountability** — every dataset has a named owner, a quality SLA, and an incident-response path, the same way production services have on-call ownership. Addresses governance pitfalls and gives the first two components someone to alert.

## **3.2 Implementation and Methodology**

*[Revisit this three-phase table once the framework is validated against real sources.]*

| Stage | Description | Key Deliverable |
| :---- | :---- | :---- |
| Phase 1 — Baseline & Contract | Inventory critical datasets; establish quality baselines per pitfall category; define contracts between top producer/consumer pairs | Data inventory, quality baseline, first contracts in place |
| Phase 2 — Instrument | Deploy observability/lineage tooling; wire contract violations and anomaly detection into alerting | Automated detection replacing manual discovery |
| Phase 3 — Assign & Operate | Assign named owners and SLAs per dataset; establish incident response and postmortems for data-quality failures the way they exist for uptime | Ownership model in production, ongoing incident tracking |

`[CITATION NEEDED — figure/chart placeholder once assets/ exists]`

## **3.3 Benefits and Outcomes**

`[CITATION NEEDED — reported outcome figures: incident/MTTR reduction, reduced rework, model-performance stability, ROI]`

---

# **4. Cross-Industry Evidence**

*[Outline stage. Vignettes are supporting evidence for the taxonomy/framework, not the spine of the argument — keep each one short. Candidates below need verification before being asserted as fact.]*

**Banking & Financial Services.** Verified anchor: OCC's $400M civil penalty against Citibank (Oct 2020) for data governance and risk-data-aggregation failures, plus a $135.6M follow-on fine (2024) for insufficient remediation. `[DRAFT PROSE NEEDED — source is verified, see research file Section 8]`

**Healthcare.** Verified anchor: de Andrade et al. (2026), *Critical Care*, systematized review of EHR data-quality issues in critical care — missing-data rates over 80% for some ICU variables, 34% of ICU medication errors EHR-related, a sepsis-detection ML model's AUC dropping from 0.76–0.83 internally to 0.63 externally due to data-quality degradation. `[DRAFT PROSE NEEDED — source is verified, see research file Section 8]`

**Retail / Technology — AI/ML pricing and targeting failures.** Verified case study: *Unity Technologies (2022)* — CEO John Riccitiello confirmed on an earnings call that ingesting bad training data from a large customer degraded its Audience Pinpointer ad-targeting model, with an estimated $110M 2022 business impact. `[DRAFT PROSE NEEDED — source is verified, see research file Section 4]`

*Zillow Offers was considered and rejected as a candidate for this section — primary sources (Zillow's CEO, a Stanford GSB analysis, the AI Incident Database) attribute its 2021 shutdown to forecasting/strategic failure, not data quality. Do not reintroduce it here.*

**Government & Public Sector.** Verified anchor: GAO-25-107469 (Sept 2025) — only 51% of 70 federal agencies completed required data-quality certifications for FY2023 procurement data; some agencies reran statistical samples until reaching a desired result rather than using valid random sampling. `[DRAFT PROSE NEEDED — source is verified, see research file Section 8]`

---

# **5. Conclusion**

*[Draft — revise once Sections 2–4 are sourced.]*

Data-quality failures are usually treated as bad luck: a one-off bug, a rogue upstream change, a model that happened to degrade. Treated individually, each incident gets a fix and a postmortem, and the next one arrives from a different direction using the same underlying gap. The six-category taxonomy in this paper argues the opposite: the failure modes are finite, they recur predictably, and they are addressable as infrastructure — contracts at the boundary, continuous observability and lineage, and named ownership — rather than refought incident by incident.

**Future Outlook**

`[Draft once framework outcomes are sourced. Candidate angle: as AI/ML systems become the primary consumer of enterprise data, the cost of the same six pitfalls compounds faster and more invisibly than it did for BI/reporting use cases — making data-quality infrastructure a precondition for reliable AI, not a parallel workstream.]`

---

# **Contact Information**

**Krishna Murthy Kodiganti**
Senior Lead Software Engineer, Capital One

For questions or to discuss further, please reach out via the contact details associated with this publication.

*The views expressed in this paper are the author's own and do not represent the views of Capital One.*

---

# **Sources**

*[Empty — populate from the research file as claims are sourced. Three-tier structure: Primary & Regulatory/Standards, Academic & Peer-Reviewed Literature, Industry Research & Vendor Sources.]*

**Primary & Regulatory / Standards Sources**

*(none yet)*

**Academic & Peer-Reviewed Literature**

*(none yet — Sambasivan et al. 2021 is the leading candidate, pending verification)*

**Industry Research & Vendor Sources**

*(none yet)*
