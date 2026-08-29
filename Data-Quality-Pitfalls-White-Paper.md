# **Data Quality Pitfalls: From Recurring Failure Modes to a Cross-Industry Prevention Framework**

**Krishna Murthy Kodiganti**
*Senior Lead Software Engineer, Capital One*

*The views expressed in this paper are the author's own and do not represent the views of Capital One.*

---

# **Abstract**

Organizations across every industry treat data-quality failures as isolated incidents: a bad report here, a failed compliance filing there, a machine learning model that quietly degrades in production. Poor data quality costs the U.S. economy an estimated $3 trillion annually (Redman, 2016), and the pattern repeats inside individual organizations: enterprises report an average cost of roughly $12.9 million per year from bad data, based on a survey of reference customers already running data-quality programs (Gartner, 2020).

These failures are not independent incidents. They recur across six failure-mode categories: structural, semantic, identity and duplication, statistical and distributional (including machine-learning-specific drift and bias), temporal, and governance. Most organizations have no infrastructure built to catch them systematically; they catch them after the fact instead, one incident at a time. This paper proposes a three-part prevention framework: data contracts at the boundary, continuous observability and lineage, and named ownership and accountability. A validation-checkpoint model makes the framework concrete by specifying when a check should run (ingress, in-flight, or egress) and how deep it should go (syntactic, internally consistent, or verified against external ground truth). Organizations that adopt comparable data-observability practices report returns as high as 358% over three years (Forrester Consulting / Monte Carlo, 2025). Those without such practices report data-quality incidents rising year over year, taking longer to detect and longer to resolve (Monte Carlo / Wakefield Research, 2023).

---

# **1. Introduction**

## **Background**

Every organization that collects, stores, or acts on data is implicitly betting that the data reflects reality closely enough to make a good decision from it. That bet fails constantly, and usually silently: a schema changes upstream and nobody downstream is told; two teams use the same column name to mean two different things; a duplicate customer record splits one person's history into two profiles; a training set drifts away from the population a model now serves in production; a nightly job fails partway through and nobody notices for three weeks. None of these are exotic. They are the ordinary condition of production data, and they occur at a comparable rate regardless of industry, team maturity, or the sophistication of the systems built on top of the data.

## **Problem Statement**

Organizations treat these failures as one-off incidents, triaged and forgotten instead of recognized as symptoms of a missing layer of infrastructure. **How should any organization, in any industry, prevent the recurring failure modes that make data untrustworthy at the point of use, instead of repeatedly firefighting them once they surface?**

---

# **2. A Taxonomy of Data Quality Pitfalls**

The six categories below are ordered from pitfalls closest to data ingestion to pitfalls rooted in organizational process. Each maps to a specific part of the prevention framework proposed in Section 3.

## **2.1 Structural Pitfalls**

Schema drift, encoding and format inconsistency, and silent truncation or type coercion at ingestion are the most immediate data-quality pitfalls, because they happen at the boundary between systems built by different teams with no shared contract. A producer changes a field's type, name, or nullability without notice; a downstream pipeline built against the old shape breaks, or worse, silently miscomputes instead of failing loudly. Sanderson (2022) frames this as engineering and data teams treating a production database as a "non-consensual API": one side changes the schema for its own reasons, and the other side finds out only after something downstream has already broken.

## **2.2 Semantic Pitfalls**

A field can pass every structural check and still be wrong in meaning. The same named metric, "monthly active users," "revenue," gets independently redefined in a BI tool, a notebook, and a dashboard, producing different "correct" numbers because the underlying logic differs: one counts pending invoices as revenue, another doesn't (dbt Labs). Unit mismatches follow the same pattern. An "amount" field stored in cents in one system and dollars in another passes type validation without complaint. The business logic that would catch the mismatch usually lives buried in someone's ETL script instead of a shared definition, which is why these pitfalls tend to surface as disagreement between departments' dashboards, not as an outright pipeline failure.

## **2.3 Identity & Duplication Pitfalls**

Fragmented or duplicate records for the same real-world entity are a special case of semantic drift: the same person or organization exists as separate, disconnected records because names, addresses, or identifiers were captured inconsistently across systems. A 2025 propensity-matched cohort study in *BMJ Quality & Safety* put a number on the harm: compared with matched patients who had no duplicate records, patients with duplicate medical records had roughly five times the adjusted risk of inpatient death, 3.5 times the risk of ICU admission, and a hospital stay 32% longer. Duplicate-record prevalence in that cohort ran 5–10% of all records reviewed.

## **2.4 Statistical & Distributional Pitfalls (including ML-specific)**

Machine learning systems add a category of pitfall that traditional reporting pipelines don't have: statistical drift between the data a model was trained on and the data it now serves in production, label noise, sampling bias, and feedback loops where a model's own outputs pollute the data used to retrain it. Sambasivan et al. (2021) interviewed 53 AI practitioners across India, East and West Africa, and the United States. 92% had experienced at least one "data cascade," a compounding downstream failure traced back to an upstream data issue, and 45% had experienced multiple cascades. Their central finding: data work is structurally undervalued relative to model work, and that undervaluing is the root cause. Sculley et al. (2015) name the specific mechanisms: entanglement, where changing any one input feature changes the behavior of others ("changing anything changes everything"), hidden feedback loops, and undeclared consumers. Data dependencies in ML systems, they argue, are as costly as code dependencies and far harder to detect. Unity Technologies put a dollar figure on the pattern: on its Q1 2022 earnings call, the company disclosed that ingesting bad training data from a large customer had degraded its Audience Pinpointer ad-targeting model, an estimated $110 million impact to the business in 2022 alone.

## **2.5 Temporal Pitfalls**

Staleness and latency are pitfalls of timing, not content. Staleness means data is consumed after it has stopped reflecting reality; latency means correct data arrives too late to act on. Uber's own data engineering team describes the cost directly: without faster downstream transformation and access, "data remains stale at the point of decision," with business impact spanning experimentation, risk detection, personalization, and operational analytics. Moving from batch to streaming processing cut their latency from hours to minutes. Out-of-order events and silent backfills compound the problem. A backfill that changes historical values after downstream consumers have already acted on the old ones breaks a basic assumption: that yesterday's numbers stay yesterday's numbers.

## **2.6 Governance & Accountability Pitfalls**

None of the preceding five pitfalls is hard to detect with the right tooling. What's consistently missing is ownership: no named data owner per dataset, no data-quality SLA, no lineage showing what depends on what, tribal knowledge substituting for documentation, and data quality treated as a one-time cleanup project instead of a continuous discipline. This is well-trodden ground for the data-management profession. DAMA International's DMBOK2 names data quality management and organizational role expectations as separate, explicit knowledge areas, and ISO's 8000 series formalizes the same split into a process reference model (ISO 8000-61), governance (ISO 8000-51), and named roles and responsibilities (ISO 8000-150). Practitioners have reached the same conclusion on their own. GoCardless adopted data contracts specifically because undocumented upstream schema changes were degrading downstream ML-model data quality, and industry writing on the "rise of data contracts" pins the underlying problem on missing ownership, not missing tooling: production databases treated as APIs nobody agreed to.

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

Section 2 lays out what goes wrong. This section lays out how to prevent it: three components, two further questions about when and how deeply to check, and three implementation phases.

## **3.1 Overview**

Three components address the six pitfall categories above as a system, not one incident at a time:

1. **Data contracts at the boundary** — a versioned, enforced interface between a data producer and consumer (schema + semantic meaning + SLA), checked in CI rather than discovered in production. Addresses structural and semantic pitfalls directly.
2. **Continuous observability and lineage** — automated monitoring for freshness, volume, schema, and distributional anomalies, plus traceability of what depends on what. Addresses temporal and statistical/distributional pitfalls, and makes governance gaps visible.
3. **Named ownership and accountability** — every dataset has a named owner, a quality SLA, and an incident-response path, the same way production services have on-call ownership. Addresses governance pitfalls and gives the first two components someone to alert.

## **3.2 Validation Checkpoints Across the Data Lifecycle**

The three components in Section 3.1 describe what to build. Two further questions determine where a given check actually catches a failure: when in the data lifecycle it runs, and how deep it goes.

### **3.2.1 When: Lifecycle Checkpoints**

Any system that accepts, transforms, or emits data has three natural points to validate it, regardless of its architecture: before the data is accepted (**ingress**), while it is being transformed (**in-flight**), and before it is handed off to whatever consumes it (**egress**). The three checkpoints are a property of how data flows, independent of the specific technology carrying it: a synchronous request/response service, an asynchronous batch job, a streaming consumer, and a scheduled ETL run all have the same three points to check. Each checkpoint catches a different slice of the Section 2 taxonomy:

| Checkpoint | What it protects against | Primarily catches |
| :---- | :---- | :---- |
| **Ingress (pre-validation)** | Malformed, incomplete, or unexpectedly-shaped data entering the system at all | Structural pitfalls (2.1), semantic pitfalls (2.2) |
| **In-flight** | Corruption or drift introduced by the system's own transformation logic | Statistical and distributional pitfalls (2.4) |
| **Egress (post-validation)** | Bad output reaching a downstream consumer before anyone notices | Temporal pitfalls (2.5), silent corruption introduced during processing |

How each checkpoint gets implemented varies by architecture. A synchronous request/response service validates at the boundary of a single call: request in, response out, with fewer internal stages to check in between. A distributed batch system like Apache Spark can declare constraints once and evaluate them as aggregation queries at every stage of a multi-step pipeline (Schelter et al., 2018). A streaming consumer validates against a schema registry as messages arrive. These three examples illustrate the same checkpoints; they don't mark the boundary of where the model applies, since the underlying principle is architecture-agnostic.

### **3.2.2 How Deep: Verification Depth**

Orthogonal to *when* a check runs is *how deep* it goes. Three depths, each answering a different question:

| Depth | Question it answers | Outcome |
| :---- | :---- | :---- |
| **Syntactic** | Is the data well-formed? | Binary pass/fail |
| **Internal consistency** | Does it agree with the rest of the organization's own data? | Binary pass/fail |
| **External ground truth** | Does it agree with reality, per an authoritative outside source? | **Confidence score, not binary** |

The third depth differs from the first two in kind, not degree. Verifying an attribute against an authoritative external source produces a graded confidence level, not a verdict. The pattern applies wherever an organization holds a claim about the world and wants to know how much to trust it, not just whether the claim is present: a person's address, a phone number's ownership, an email's deliverability, a bank account's ownership, a business's registration status, a document's authenticity. NIST formalizes the same graded-confidence pattern for identity proofing through its Identity Assurance Levels (IAL1–IAL3), each level requiring progressively more rigorous evidence and validation against authoritative sources instead of a simple yes/no. USPS CASS certification does the same for address-matching, grading software against ZIP+4, delivery-point, and carrier-route accuracy instead of treating an address as simply valid or invalid. Both are concrete instances of the general pattern; neither marks its outer edge. A standards-body anchor for other attribute types (phone ownership, bank account ownership, business registration) hasn't turned up yet. That gap is left open here rather than papered over.

External ground-truth verification introduces four consequences the first two depths don't have, and the framework needs an explicit answer for each:

1. **A score needs a policy, not just a threshold.** A mid-range confidence score is neither a pass nor a fail; it needs a defined path: auto-accept above one threshold, auto-reject below another, human review in between. It's the same three-way accept/reject/review decision that probabilistic record linkage has used since Fellegi and Sunter formalized it in 1969.
2. **Confidence decays.** A claim confirmed valid at one point (an address, a phone number, a business's registration status) may not stay valid. That's the Temporal pitfall (2.5) surfacing inside the framework itself, and it argues for periodic re-verification instead of a one-time check at intake.
3. **It has a cost and a latency, so it forces a placement decision.** A synchronous call to an external verification service blocks the request it's protecting; an asynchronous, post-acceptance enrichment avoids that cost but means bad data is briefly live before it's flagged. Which one is appropriate depends on the cost of being briefly wrong versus the cost of the added latency.
4. **It moves the trust boundary; it doesn't eliminate it.** A confidence score is only as reliable as the third party's own data quality, so external verification trades a vendor's data-quality problem for the organization's own instead of removing the dependency. It also raises a governance question the first two depths don't: sending PII to an external verification service is a data-handling decision (consent, data minimization, cross-border transfer) that belongs under the Governance pitfall (2.6), not a purely technical integration detail.

## **3.3 Implementation and Methodology**

The three components and the checkpoint model translate into three implementation phases, following the same discipline organizations already apply to production-service reliability:

| Stage | Description | Key Deliverable |
| :---- | :---- | :---- |
| Phase 1 — Baseline & Contract | Inventory critical datasets; establish quality baselines per pitfall category; define contracts between top producer/consumer pairs | Data inventory, quality baseline, first contracts in place |
| Phase 2 — Instrument | Deploy observability/lineage tooling; wire contract violations and anomaly detection into alerting; decide checkpoint placement and verification depth per dataset (Section 3.2) | Automated detection replacing manual discovery |
| Phase 3 — Assign & Operate | Assign named owners and SLAs per dataset; establish incident response and postmortems for data-quality failures the way they exist for uptime | Ownership model in production, ongoing incident tracking |

## **3.4 Benefits and Outcomes**

Organizations that adopt comparable data-observability and contract practices report measurable gains. A 2025 Forrester Consulting Total Economic Impact study of a data-and-AI observability platform found 358% ROI over three years, driven by reclaimed data-personnel hours, avoided revenue loss from data downtime, and improved AI/ML model efficacy. The cost of skipping this is equally measurable, and it's heading in the wrong direction: a 2023 industry survey of 200 data professionals found monthly data incidents rising from 59 to 67 year over year, average time-to-resolution up 166% to 15 hours, and average revenue impacted by data downtime up to 31%, with over half of respondents reporting a 25%-or-greater hit.

---

# **4. Cross-Industry Evidence**

The six pitfall categories, and the framework proposed to address them, apply beyond any single sector. Four brief vignettes show the same underlying pattern surfacing under different names and different regulatory regimes.

**Banking & Financial Services.** In October 2020, the Office of the Comptroller of the Currency assessed a $400 million civil penalty against Citibank, N.A. for a "long-standing failure to establish effective risk management and data governance programs and internal controls," citing deficiencies specifically in data governance, risk data aggregation, and regulatory reporting. Citibank was fined an additional $135.6 million by the OCC and Federal Reserve in 2024 for insufficient progress remediating the same issues. The pattern isn't confined to one bank: a 2023 Basel Committee review of its own 2013 risk-data-aggregation principles (BCBS 239) found only 2 of 31 globally systemic banks fully compliant, a decade after the principles were published.

**Healthcare.** A 2026 systematized review in *Critical Care* of 29 studies on electronic health record data quality in intensive care found missing-data rates exceeding 80% for some ICU variables and EHR-related medication errors accounting for 34% of all ICU medication errors, a third of them life-threatening. The same review found a sepsis-detection machine learning model's performance dropping from an internal AUC of 0.76–0.83 to 0.63 under external validation, attributed to data-quality degradation between the environment the model was trained in and the environment it was deployed to. It's the statistical/distributional pitfall from Section 2.4, surfacing directly in patient-safety terms.

**Retail & Technology.** Unity Technologies' Q1 2022 earnings call is a clean, on-the-record instance of the same failure in an advertising-technology context: CEO John Riccitiello disclosed that the company had "lost the value of a portion of our data, training data due in part to us ingesting bad data from a large customer," with an estimated $110 million impact to the business in 2022.

**Government & Public Sector.** A September 2025 U.S. Government Accountability Office report found only 51% of 70 federal agencies had completed required data-quality certifications for fiscal year 2023 procurement data, that none of the 24 CFO Act agencies fully met their reporting obligations, and that some agencies reran statistical samples until reaching a desired result instead of using valid random sampling. It's a governance pitfall (2.6) playing out at national scale, not a technical glitch.

---

# **5. Conclusion**

Data-quality failures are usually treated as bad luck: a one-off bug, a rogue upstream change, a model that happened to degrade. Handled individually, each incident gets a fix and a postmortem, and the next one arrives from a different direction through the same underlying gap. The failure modes are finite, though, and they recur predictably. Contracts at the boundary, continuous observability and lineage, and named ownership turn them into something an organization builds once, instead of refighting incident by incident.

**Future Outlook**

As AI and ML systems become the primary consumers of enterprise data, the same six pitfalls compound faster and more invisibly than they did for BI and reporting. A degraded dashboard is visible to whoever looks at it. A degraded model just ships worse decisions until someone notices the outcome, long after the cause has passed. Standards bodies already treat data quality as inseparable from AI risk: NIST's AI Risk Management Framework threads data lineage, representativeness, and drift through its core risk-management functions instead of isolating them in a single section. Organizations that build data-quality infrastructure as a precondition for AI, not a parallel initiative, will be the ones whose AI systems are trustworthy enough to put in front of a regulator, a customer, or a patient.

---

# **Contact Information**

**Krishna Murthy Kodiganti**
Senior Lead Software Engineer, Capital One

For questions or to discuss further, please reach out via the contact details associated with this publication.

*The views expressed in this paper are the author's own and do not represent the views of Capital One.*

---

# **Sources**

Organized into three tiers by evidentiary weight: primary, regulatory, and standards sources; peer-reviewed academic literature; and industry research or vendor publications. The tiering is meant to make sourcing quality visible rather than to imply lower tiers are unreliable — see the note at the end of this section.

**Primary, Regulatory & Standards Sources**

1. Office of the Comptroller of the Currency (2020). *OCC assesses $400 million civil money penalty against Citibank.* https://www.occ.gov/news-issuances/news-releases/2020/nr-occ-2020-132.html
2. Banking Dive (2024). *Citi hit with $135.6M fine over risk management, data governance shortfalls.* https://www.bankingdive.com/news/citi-risk-management-data-governance-OCC-fed/586642/
3. Basel Committee on Banking Supervision (2013). *Principles for effective risk data aggregation and risk reporting (BCBS 239).* https://www.bis.org/publ/bcbs239.pdf
4. U.S. Government Accountability Office (2025). *Federal Spending Transparency: Actions Needed to Help Ensure Procurement Data Quality.* GAO-25-107469. https://www.gao.gov/products/gao-25-107469
5. Unity Software Inc. (2022). *Q1 2022 Earnings Call Transcript*, May 11, 2022 (The Motley Fool). https://www.fool.com/earnings/call-transcripts/2022/05/11/unity-software-inc-u-q1-2022-earnings-call-transcr/
6. National Institute of Standards and Technology (2023). *AI Risk Management Framework (AI RMF 1.0).* https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-ai-rmf-10
7. National Institute of Standards and Technology (2025). *SP 800-63A, Digital Identity Guidelines — Enrollment and Identity Proofing.* https://pages.nist.gov/800-63-4/sp800-63a.html
8. International Organization for Standardization (2016). *ISO 8000-61:2016 — Data quality — Part 61: Data quality management: Process reference model.* https://www.iso.org/standard/63086.html
9. International Organization for Standardization (2022). *ISO 8000-150:2022 — Data quality — Part 150: Data quality management: Roles and responsibilities.* https://www.iso.org/standard/80753.html
10. International Organization for Standardization (2023). *ISO 8000-51:2023 — Data quality — Part 51: Data governance.* https://www.iso.org/standard/78708.html
11. U.S. Postal Service (2025). *CASS™ (Coding Accuracy Support System) certification.* https://postalpro.usps.com/certifications/cass

**Academic & Peer-Reviewed Literature**

12. Fellegi, I.P. & Sunter, A.B. (1969). *A Theory for Record Linkage.* Journal of the American Statistical Association, 64(328), 1183–1210. https://doi.org/10.1080/01621459.1969.10501049
13. Sculley, D., Holt, G., Golovin, D., Davydov, E., Phillips, T., Ebner, D., Chaudhary, V., Young, M., Crespo, J-F., & Dennison, D. (2015). *Hidden Technical Debt in Machine Learning Systems.* NeurIPS 2015. https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems
14. Schelter, S., Lange, D., Schmidt, P., Celikel, M., Grafberger, A., & Biessmann, F. (2018). *Automating Large-Scale Data Quality Verification.* Proceedings of the VLDB Endowment, 11(12). https://www.vldb.org/pvldb/vol11/p1781-schelter.pdf
15. Sambasivan, N., Kapania, S., Highfill, H., Akrong, D., Paritosh, P., & Aroyo, L.M. (2021). *"Everyone wants to do the model work, not the data work": Data Cascades in High-Stakes AI.* CHI '21. https://doi.org/10.1145/3411764.3445518
16. BMJ Quality & Safety (2025). *Double trouble: a propensity-matched cohort study evaluating the associations between duplicate medical records and patient outcomes.* https://doi.org/10.1136/bmjqs-2025-019112
17. de Andrade, J.B.C. et al. (2026). *Discovery of data quality issues in electronic health records: profound consequences for critical care medicine applications — a systematized review.* Critical Care, 30:19. https://doi.org/10.1186/s13054-025-05677-0

**Industry Research & Vendor Sources**

18. Redman, T.C. (2016). *Bad Data Costs the U.S. $3 Trillion Per Year.* Harvard Business Review. https://hbr.org/2016/09/bad-data-costs-the-u-s-3-trillion-per-year
19. Dataversity (2020). *Putting a Number on Bad Data* (discussing Gartner's 2020 Magic Quadrant for Data Quality Solutions). https://www.dataversity.net/articles/putting-a-number-on-bad-data/
20. DAMA International (2017/2024). *DAMA-DMBOK2: Guide to the Data Management Body of Knowledge*, 2nd Edition. Technics Publications. https://technicspub.com/dmbok2/
21. Sanderson, C. (2022). *The Rise of Data Contracts — And Why Your Data Pipelines Don't Scale.* https://dataproducts.substack.com/p/the-rise-of-data-contracts
22. Jones, A. (2021). *Improving Data Quality with Data Contracts*, GoCardless Tech blog. https://medium.com/gocardless-tech/improving-data-quality-with-data-contracts-238041e35698
23. dbt Labs. *How the dbt Semantic Layer Works.* https://www.getdbt.com/blog/how-the-dbt-semantic-layer-works
24. Forrester Consulting / Monte Carlo Data (2025). *The Total Economic Impact™ of Monte Carlo's Data + AI Observability Platform.* https://tei.forrester.com/go/montecarlo/dataaiobservabilityplatform
25. Monte Carlo Data / Wakefield Research (2023). *The State of Data Quality, 2023.* https://montecarlo.ai/state-of-data-quality/

*Note: sources 18–25 are industry, vendor, or vendor-commissioned publications rather than peer-reviewed research or primary regulatory documents; figures from that tier are presented as commonly cited industry estimates, not independently audited data. Two of them (24 and 25, both Monte Carlo–affiliated) are flagged specifically because Monte Carlo is a data-observability vendor with a direct commercial interest in the framework this paper proposes — the underlying methodology (Forrester's TEI framework for #24, a fielded 200-respondent survey for #25) is disclosed, but neither is an independently audited study. Where possible, primary regulatory and standards sources (OCC, GAO, NIST, ISO, USPS) and peer-reviewed academic literature (sources 12–17) are cited directly and prioritized.*
