# Constitution — exec-company-org-chart

Governing principles for this project. Every spec, plan, and implementation decision
must comply with these. If a proposed change conflicts with a principle, the change
loses — or the constitution is amended deliberately, never silently.

## Article I — Truth over completeness
Never fabricate, infer, or "fill in" an executive, title, or reporting line. A
smaller chart of confirmed people beats a fuller chart with guesses. If the
leadership page cannot be found or read, the skill says so — it does not invent
a C-suite.

## Article II — Provenance on every node
Every person on the chart carries a citation: the URL of the page they were found
on. Every chart carries an as-of date. No source, no node.

## Article III — Official sources ONLY
*(Amended in Gate 3 — original allowed labeled secondary sources; owner ruled them out.)*
The company's own published material (its website pages and its own published
documents) is the sole acceptable source. Third-party data — LinkedIn, news,
aggregators, wikis — is banned entirely: company dynamics change in real time, and
second-guessing who's in or who's out from outside sources is exactly the failure
this skill exists to avoid. The truth always comes back to the official page; when
the official page has no answer, the answer is the honest fallback, not a search
party.

*(Second amendment, Gate 6 — owner ruling after live testing.)* **Company-authored
regulatory filings** (SEC filings, exchange disclosures, AGM notices) count as
official material — the company wrote them, even when a regulator hosts them. They
are a **fallback tier only**, used when the company's own domain yields nothing
readable, and they carry a mandatory caveat: filings often list only officers the
regulation requires, so the roster may be incomplete — the output must say so.

## Article IV — Level-1 scope is a feature
The chart covers the top of the company only: CEO (or equivalent) and their direct
executive team. Deeper levels are out of scope by design — public sources are not
reliable below level 1, and Article I forbids guessing.

## Article V — Zero-dependency output
Output must render anywhere markdown renders: Mermaid diagram + markdown table.
No Python, no image generation, no external services required to view the result.

## Article VI — Simple to invoke
The user provides at minimum a company name. Everything else the skill figures out
or asks. No CSVs, no data preparation, no configuration required.

## Article VII — Honest freshness
Leadership changes. The skill states when the source page was accessed and, when
detectable, flags stale or conflicting information rather than silently picking
a side.
