# Spec 001 — exec-company-org-chart

**Status:** Draft — pending Clarify (Gate 3)
**Governed by:** `memory/constitution.md` (all seven articles apply)

## 1. Problem statement

People constantly need to know "who runs this company?" — salespeople mapping a
prospect, candidates researching an employer, founders studying a competitor,
investors scanning a portfolio target, journalists checking a fact. The answer is
public (companies publish leadership pages) but scattered, unstructured, and tedious
to assemble by hand.

The existing org-chart skill on skills.sh requires the user to ALREADY have the data
(a CSV of names and managers). In the common case the user has nothing but a company
name. This skill closes that gap: **company name in → sourced level-1 org chart out.**

## 2. Users & core user stories

- **US1 (seller/BD):** As someone preparing outreach, I give a company name and get
  the CEO and executive team with correct titles, so I can map stakeholders in
  under a minute.
- **US2 (job seeker):** As a candidate, I ask for a company's leadership before an
  interview and get a chart I can trust, with links to verify each person myself.
- **US3 (analyst/investor):** As a researcher, I need the as-of date and sources on
  every chart so I can cite it in my own work.
- **US4 (skeptic):** As any user, when the skill can't find reliable data, I want it
  to tell me that plainly instead of showing me a plausible-looking wrong chart.

## 3. Inputs

- **Required:** a company name or account name (free text: "Zerodha", "Amadeus").
- **Optional:** a **website domain — the grounding anchor.** When provided, the
  skill must not deviate from it: all leadership data comes from (or is anchored to)
  that domain's entity.
- **Location is NOT an input.** Region qualifiers ("Costco Australia") always create
  conflicts and are rejected as a concept:
  - Domain given (e.g. `https://www.costco.com.au/`) → chart THAT entity, exactly
    as its site publishes leadership.
  - Name only ("Costco") → resolve to the **main/global HQ entity** — never a
    regional subsidiary, never a guessed region.
- Input scope may grow (further input rules to be clarified in later Gate-3 rounds);
  the grounding hierarchy is fixed: **domain > name > nothing else.**

## 4. Output (what the user receives)

One response containing, in order:
1. **Company header** — resolved company name + official site + as-of date.
2. **Org chart** — CEO (or equivalent top role) at the top, direct executive team
   below, rendered as a diagram that displays in any markdown surface.
3. **Roster table** — one row per person: name, exact title, source URL.
4. **Notes** — anything honest the user needs: people the skill could NOT verify,
   conflicting sources, page-freshness warnings (per Articles I, II, VII).

## 5. Behavior rules (user-observable)

- **B1:** Only people found on acceptable sources appear on the chart (Article I, III).
- **B2:** Every person carries a source link (Article II).
- **B3:** Chart depth is exactly level 1: the top role + their executive team.
  No middle management, no board of directors by default (Article IV).
- **B4 (complexity fallback):** In case of ANY doubt — company unresolvable,
  ambiguous, no discoverable leadership info, conflicting data — the skill returns
  an honest complexity fallback: what it searched, what it found or didn't, and a
  suggestion (provide the website domain / check the company site manually).
  **A wrong detail is a failure; an honest empty answer is not.** It may ask one
  sharp clarifying question when conversation allows, but it never outputs an
  unsourced or guessed chart (US4, Article I).
- **B5:** Board of directors and advisors are excluded unless the user explicitly
  asks. **Founders are INCLUDED** — in new/small startups the founders often ARE
  the leadership; if the company presents founders as its team, they are the chart.
- **B6:** Works for public and private companies alike. Leadership disclosure
  varies by region and entity type, and the skill must handle both major forms:
  - **Web pages** — leadership / executive committee / management team pages
    (common for US/EU public and private companies).
  - **PDF documents** — annual reports, governance reports, executive summaries
    (common in China, Japan, APAC, India, and government entities). PDF-published
    leadership is in scope, not an excuse to return empty.
- **B7 (diverse role vocabulary):** "Executive team" is recognized across naming
  conventions — Executive Committee, Management Board, Managing Team, Senior
  Leadership, Group Executive, Board of Management; top roles vary by region
  (Japan: Managing Officer / Managing Director rather than CEO; Australia:
  Executive General Manager; etc.). The operation must not assume US C-suite
  vocabulary. **The company's own designation wins — always.**
- **B8 (single-root, page-defined hierarchy):** The chart is what the page says,
  structured under ONE root: the person the company presents as top leader
  (whatever the title). Everyone else listed becomes a direct child of that root.
  The skill NEVER infers reporting lines on its own; only if the official page
  itself explicitly presents a hierarchy does the chart mirror it. Users can
  always correct locally — a flat-but-true chart beats a structured guess.
- **B9 (documents are in scope, financial deep-dives are not):** Site navigation,
  leadership-section lookup, and PDF extraction are core capability. Annual/
  financial reports are a **last resort only** — they're org-specific rabbit holes
  no general system can promise to navigate; use them for basic executive-team
  extraction when nothing else exists, otherwise fall back honestly (B4). The
  target is a solid **basic level-1 executive mapping**, not a filings parser.

## 6. Edge cases the spec must survive

- **E1:** Ambiguous name (multiple companies called "Apollo") → the fix is the
  grounding anchor: request the website domain, or resolve to the clearly dominant
  main entity if one exists. Doubt → B4 complexity fallback, never a guess.
- **E2:** No official leadership page OR document exists (common for small private
  cos) → per B4: honest empty-handed answer with a manual-lookup suggestion.
  **No secondary-source fallback of any kind** (Article III as amended).
- **E3:** Co-CEOs / co-founders sharing the top role → chart supports multiple
  roots side by side.
- **E4:** Conglomerates / holding companies ("Tata", "Orkla") → **the website
  decides.** Ground on the (given or resolved-main) domain and chart the leadership
  that entity publishes (a holding company's own executive team is a valid chart).
  If the name doesn't resolve to a website with discoverable executive info →
  return the B4 fallback ("couldn't resolve — check manually or give me the
  domain"), never a guessed pick among subsidiaries.
- **E4b:** Regional domain explicitly given (`costco.com.au`) → that entity IS the
  subject; chart its published leadership without "correcting" to global HQ.
- **E5:** Recent leadership change (page not yet updated / news contradicts page) →
  show the official page's version, flag the conflict in Notes (Article VII).
- **E6:** Non-English leadership pages → still extract; keep titles in original
  language with an English gloss where confident.
- **E7:** Very large executive teams (20+ direct reports listed) → include all;
  never truncate silently.
- **E8:** Leadership published only inside PDFs (annual report, governance report —
  typical for China/Japan/APAC/India/government entities) → locate and read the
  document; extract the executive section; cite the PDF URL (and page/section when
  possible) as the source.
- **E9:** Government entities / public-sector bodies → same rules; their
  "executive" layer is the published leadership (secretary/director-general/board
  of management as published), sourced from official sites or PDFs.

## 7. Success criteria (how we judge "it works")

- **S1:** For a well-known public company, a correct, fully-sourced level-1 chart
  from just the name — zero follow-up questions needed.
- **S2:** Zero fabricated people or titles across all tests — including the E2
  no-data case (measured: every node has a working source URL).
- **S3:** Output renders correctly in plain markdown (diagram + table) with no
  external tooling.
- **S4:** The E1/E4 ambiguity cases produce the honest complexity fallback (or one
  sharp question when conversation allows) — never a wrong guess.
- **S5:** A PDF-disclosure company (E8) yields a sourced chart citing the document.
- **S6:** A regional domain input (E4b) yields that entity's leadership — the skill
  does not "helpfully" switch to global HQ.
- **S7:** A founder-led startup whose site lists founders as the team yields those
  founders as the chart (B5).

## 8. Non-goals (explicitly out)

- Full org trees below level 1 (Article IV)
- Headcount, salaries, org-size estimates
- Continuous monitoring / change alerts
- Enriching people with emails, phones, socials (adjacent product, not this skill)
- Historical org charts ("who ran X in 2019")
