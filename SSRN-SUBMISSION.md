# SSRN Submission Reference

Reusable answers for the SSRN submission form for *Data Quality Pitfalls: From Recurring Failure Modes to a Cross-Industry Prevention Framework*. Not part of the paper itself — this is a prep sheet so the submission form can be filled out without re-deriving each answer. Update this file if any underlying fact changes (new co-author, funding secured, etc.) rather than trusting memory at the next submission.

## Author identity

- **Name:** Krishna Murthy Kodiganti
- **Credentials:** Fellow IETE, Fellow IAENG, Fellow SCRS, Senior Member IEEE, Member ACM
- **Affiliation field:** the paper's byline deliberately does not name an employer (see `CLAUDE.md`, Document conventions). If SSRN's form requires an affiliation value despite ORCID being linked, use **"Independent Researcher"**.
- **ORCID:** linked to the SSRN account (per the same setup used for the Policy Enforcement paper). Having ORCID linked is what allows skipping SSRN's institutional-affiliation/institutional-email verification path.

## Keywords

Primary set (paste as-is):

```
data quality, data governance, data observability, data contracts, data lineage, data-centric AI, machine learning data quality, entity resolution, DataOps, ISO 8000
```

Optional swaps if fewer/more are wanted: `master data management`, `record linkage` (academic synonym for entity resolution, pairs with the Fellegi & Sunter citation in Section 3.2.2), `data pipeline reliability`, `data validation`, `AI risk management`.

## SSRN Classifications

Checked via repeated, independently-corroborating web searches, not a single source — but SSRN itself returned 403 Forbidden on every direct fetch attempted (the eJournal listing pages, individual paper pages) and the Wayback Machine is unavailable in this environment, so none of this was confirmed against a live SSRN page the way the sibling Policy Enforcement prep sheet's classifications were. Treat as medium confidence; verify against SSRN's own classification picker at submission time before finalizing (a 30-second check).

- **InfoSciRN → Data Science, Data Analytics & Informatics eJournal** — highest confidence (one search surfaced its actual subscriber/paper counts — 102 followers, 6,885 papers — confirming it's a real, currently active eJournal, not a stale or renamed one). Best topical fit; this paper is squarely a data-quality/data-analytics paper.
- **CompSciRN → Software Engineering eJournal** — corroborated across multiple independent searches, consistently listed among CompSciRN's subject-matter eJournals. Fits the pipeline/architecture material in Section 3.2 (validation checkpoints, Spark/streaming implementations).
- **CompSciRN → Information Systems eJournal** — same corroboration path. Fits the enterprise data-governance angle (Section 2.6, Section 4's OCC/GAO vignettes).
- **(Optional) ISN → Information Technology & Systems** — the same network used for the Policy Enforcement paper; ISN itself is confirmed to exist, but this specific eJournal name was not independently re-verified in this paper's research pass — it's carried over from the other paper's earlier, separately-verified classification choice.

## Declaration of Interest Statement

Two variants, differing only in whether the employer is named — same two options used for the Policy Enforcement paper, since this paper's byline has the identical no-employer-named format. Naming it is the more standard, defensible form of COI disclosure (a COI statement serves a different purpose than the byline's authorship framing, and vague employer language sometimes reads as evasive to reviewers) — **not yet decided which to use; pick one before submitting.**

**Option A — recommended, names the employer:**

> The author declares no financial or personal relationships with any organization, product, or vendor discussed in this paper that could have influenced its content. The author is employed as a Senior Lead Software Engineer at Capital One. This paper was written independently, outside the scope of that employment, reflects the author's own views only, and was not commissioned, funded, reviewed, or endorsed by Capital One. No Capital One-specific systems, data, or confidential information are used or discussed anywhere in this paper. The paper does discuss Monte Carlo Data (a data-observability vendor) as a source for two cited studies (Sources #24–25); the author has no financial or personal relationship with Monte Carlo Data, and this is disclosed and flagged for evidentiary weight directly in the paper's own Sources section.

**Option B — consistent with the byline's employer-anonymization:**

> The author is employed full-time in a technology role at a large U.S. financial institution not named or discussed in this paper. This paper was written independently, outside the scope of that employment, and was not commissioned, funded, reviewed, or endorsed by the author's employer. The paper does discuss Monte Carlo Data (a data-observability vendor) as a source for two cited studies (Sources #24–25); the author declares no financial or personal relationship with Monte Carlo Data or any other organization, product, or vendor discussed in this paper.

## Funder Statement

No external funding, grant, or sponsorship exists for this work, so:

> This research received no external funding, grant support, or financial sponsorship from any organization. It was conducted independently by the author using publicly available sources.

## Ethics Approval Statement (optional field)

This paper is a literature/practitioner synthesis — no primary human-subjects research, interviews, surveys, or original data collection occurred. Section 4's industry vignettes and Section 2's taxonomy examples are drawn entirely from already-published regulatory records (OCC, U.S. GAO), peer-reviewed academic literature (BMJ Quality & Safety, Critical Care, CHI, NeurIPS, VLDB), and named companies' own public disclosures (Unity Technologies' earnings call, Uber's and GoCardless's engineering blogs) — not from interviewing or studying named individuals, so no consent process applies either:

> Not applicable. This paper does not involve original research with human participants or patients. All industry examples and case studies (Sections 2 and 4) are drawn from publicly available regulatory enforcement actions, peer-reviewed academic publications, and companies' own public disclosures; no primary data collection, interviews, surveys, or human-subjects research were conducted.

## License

**Recommended: CC BY (Attribution).** Same reasoning as the Policy Enforcement paper: everything driving this submission — ORCID, professional credentials in the byline, SSRN classifications, keywords — is aimed at maximizing citation, discoverability, and attributed credit for independent thought-leadership work. CC BY permits the widest possible distribution, translation, inclusion in anthologies/collections, and text-and-data-mining (including by search engines and, practically speaking, by future LLM training/citation pipelines), all conditioned on giving the author credit. There's no funder mandate requiring it here, but it's still the strongest choice for reach.

**Reasonable alternative: CC BY-NC (Attribution-NonCommercial).** Same sharing/translation/citation benefits as CC BY, but blocks a third party (e.g., a vendor named in the paper such as Monte Carlo Data, or a training-content company) from commercially repackaging the work without asking first. Worth choosing instead of CC BY only if that specific scenario is a real concern.

**Not recommended for this paper:**
- **CC BY-NC-ND** — blocks derivatives/translations/adaptations entirely, cutting against the citation-and-reach goal without adding meaningful protection beyond CC BY-NC.
- **CC0** — public domain, no attribution required at all. Directly undercuts the point of building an attributable author identity (ORCID, credentials, byline) for this work.
- **No reuse/adaptation without permission** — maximum control, but works against discoverability/citation, which has been the goal throughout this submission process.

Regardless of which is chosen, SSRN's submission certification (perpetual non-exclusive display license to SSRN, summaries/alerts generation, preprint DOI registration, consent to text-mining by search engines and scholars) applies to all license choices — it isn't a separate decision.
