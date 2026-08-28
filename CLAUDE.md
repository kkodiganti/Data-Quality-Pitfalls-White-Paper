# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is a content repository, not a software project — there is no build, lint, or test tooling. It holds a single white paper and its supporting research, authored by Krishna Murthy Kodiganti (Senior Lead Software Engineer, Capital One), written as an independent thought-leadership piece — not an official Capital One publication.

## Status

**Scaffold stage.** The paper file currently holds the section skeleton, a drafted thesis/abstract, and an outlined taxonomy + framework — not a fully sourced final draft. Claims that need a citation are marked `[CITATION NEEDED]` inline; do not remove that marker without replacing it with an actual numbered source. Do not silently invent statistics to fill these gaps — either research a real source (see the research file) or leave the marker in place.

## Files

- `Data-Quality-Pitfalls-White-Paper.md` — the white paper itself. Working title: *Data Quality Pitfalls: From Recurring Failure Modes to a Cross-Industry Prevention Framework*. Thesis: most data-quality failures are not random bad luck but symptoms of missing infrastructure (no contracts at ingestion, no continuous observability/lineage, no named ownership) — and the same taxonomy of failure modes recurs whether the downstream consumer is a compliance report, a BI dashboard, or an ML training set.
- `Data-Quality-Pitfalls-White-Paper-Research.md` — the sourcing backbone: compiled links and figures, organized by taxonomy category (mirrors the white paper's Section 2 structure rather than by industry, since the taxonomy — not industry vertical — is the primary organizing axis of this paper).
- `assets/` — not created yet. Will hold chart SVGs once the paper's figures are decided (see Chart conventions below for the style to follow when they're added).

## Document conventions

- **Numbered citations must stay in sync.** The `# Sources` section is a numbered list split into three tiers — Primary & Regulatory/Standards, Academic & Peer-Reviewed Literature, Industry Research & Vendor Sources — in that order. When adding or removing a source, update both this file and the research file, re-verify numbering/links match, and place new sources in the correct tier.
- **Sourcing quality: aim for primary and peer-reviewed where possible.** Prefer, in order: peer-reviewed literature (e.g., Sambasivan et al., "Data Cascades in High-Stakes AI," CHI 2021), named public case studies with primary reporting (e.g., Unity Technologies' 2022 earnings-call disclosure), and standards bodies (ISO 8000, DAMA-DMBOK, NIST AI RMF) over vendor-blog restatements of the same figures. Vendor sources are fine as a last resort but should be flagged in the Sources section's closing note, not presented as equivalent-weight evidence.
- **Byline and disclaimer are required together.** Any published version of the white paper must carry both the author byline (*Krishna Murthy Kodiganti, Senior Lead Software Engineer, Capital One*) and the disclaimer that views are the author's own and don't represent Capital One — title block, Contact Information section, and any excerpt used elsewhere.
- **Cross-industry framing, but taxonomy-first structure.** This paper organizes its core argument by failure-mode taxonomy (Section 2) and only uses industry vignettes as supporting evidence (Section 4) that the taxonomy holds across sectors — don't let Section 4 become the spine of the argument.
- **No embedded images.** Real chart/diagram assets live in `assets/` as standalone SVG files, linked via plain `![alt text](assets/...)`. Never inline base64 image data in the markdown.
- **Tone: no "agentic"/AI-essay register.** No meta-referential framing ("this paper argues/shows"), no "not X, it's Y" rhetorical contrast constructions, no repeated-word rhetorical cadences, no heavy em-dash-aside overuse. State claims directly (declarative, third-person); prefer commas/colons/separate sentences to stacked em-dashes.
- **Governing terminology.** "Data quality" is the umbrella term. Distinguish explicitly on first use: *data quality* (fitness for use of data at rest or in motion), *data observability* (continuous monitoring for freshness/volume/schema/distribution anomalies), *data governance* (ownership, accountability, and policy), *data contract* (a versioned, enforced interface between a data producer and consumer). Don't introduce a fifth overlapping term without adding it to that same definition sentence.

## Chart conventions (for when `assets/` is created)

Categorical palette blue `#2a78d6` / orange `#eb6834` / aqua `#1baf7a` in fixed order, chart surface `#fcfcfb`, primary ink `#0b0b0b`, secondary ink `#52514e`, muted `#898781`, gridline `#e1e0d9`, light-mode only (static document asset, not an interactive artifact). Multi-line SVG text must use **separate `<text>` elements with explicit `y` per line**, not `<tspan dy="...">` — some lightweight SVG rasterizers (e.g. macOS `sips`) don't honor `tspan`/`dy` line breaks and silently overlap lines. Validate with `sips -s format png in.svg --out out.png` and read back the PNG (preserves true `viewBox` pixel dimensions); don't trust `qlmanage -t` for overflow checks, it renders into a fixed square thumbnail and can crop content that's actually within the `viewBox`.

## Open TODOs / gaps

- **Research file is sourced; the paper body isn't yet.** `Data-Quality-Pitfalls-White-Paper-Research.md` now has a verified source for every Section 2 taxonomy category, the framework's outcome evidence, and all four cross-industry vignettes (see its own status line for exactly which entries are `[VERIFIED]` vs. still `[COULD NOT FULLY VERIFY]`). The paper body's `[CITATION NEEDED]` markers have not been replaced yet — that's the next pass: pull the verified entries in, write the prose, and promote sources into the paper's numbered `# Sources` list.
- **Zillow Offers was considered and rejected as a case study**, not just left unverified. It was checked against Zillow's own CEO statement, a Stanford GSB analysis, and the AI Incident Database — all three attribute the 2021 shutdown to forecasting/strategic failure, not data quality. Do not reintroduce it as a data-quality example. Unity Technologies (2022) is verified and confirmed as a strong, on-the-record replacement (research file Section 4).
- **A few research-file entries still need a second pass** before being cited as fact: the Airbnb "Visualizing Data Timeliness" post (403'd on fetch), the ResearchGate schema-versioning literature review (paywalled), ISO 8000-51's full abstract (403'd, title only confirmed), and GAO-26-108850 (flagged as a lead, not yet opened).
- **Figures/charts not decided.** No chart topics have been committed to yet — decide alongside the evidence section once the paper body is sourced, rather than designing charts around a thesis-not-yet-checked-against-sources.
- **Title not finalized.** "Data Quality Pitfalls: From Recurring Failure Modes to a Cross-Industry Prevention Framework" is a working title — revisit once the framework section is fleshed out.
