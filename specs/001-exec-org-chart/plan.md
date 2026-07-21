# Plan 001 — exec-company-org-chart

**Status:** Draft — pending owner approval (Gate 4)
**Implements:** `spec.md` under `memory/constitution.md`

## 1. What we're actually shipping

An **agent skill** — a standalone public GitHub repo whose `SKILL.md` teaches any
capable agent (with web search + page/PDF fetch tools) the full procedure. No code,
no server, no dependencies: the skill IS the procedural knowledge (Article V).

```
exec-company-org-chart/
├── SKILL.md            # the skill — procedure, rules, output templates
├── README.md           # humans: what it does, install, example output
├── LICENSE             # MIT
├── memory/             # constitution (SDD artifact, kept in repo)
└── specs/              # spec + plan + tasks (SDD artifacts, kept in repo)
```

Install: `npx skills add asheSky/exec-company-org-chart`

Keeping the SDD artifacts in the repo is deliberate: the skill was spec-driven and
the repo shows it — a differentiator on skills.sh where most skills are vibe-published.

## 2. The procedure SKILL.md encodes (5 phases)

### Phase A — Ground (resolve the entity)
1. **Normalize the input first:** account names carry CRM noise — strip segment
   tags and suffixes ("Costco Wholesale AU — Enterprise" → "Costco Wholesale";
   legal suffixes like Inc/Ltd/GmbH are kept only if they disambiguate).
2. Domain provided → that entity is the subject, full stop (E4b).
3. Name only → resolve to the main/global HQ official site via web search.
   **Confidence rule:** exactly one dominant company matches the name → proceed.
   Two or more plausible, distinct companies (the "Delta" problem: airline /
   faucets / dental) → do NOT pick — B4 fallback or one clarifying question.
   Regional/subsidiary domains are never acceptable resolutions unless given.
4. Cannot resolve confidently → complexity fallback (B4), suggest providing domain.

### Phase B — Discover (find the leadership source on the official property)
Try in order, stop at first hit that lists people:
1. **Direct site paths:** `/leadership`, `/about/leadership`, `/company/leadership`,
   `/about-us/management`, `/management`, `/team`, `/about/team`,
   `/executive-committee`, `/governance`, `/investor-relations` (leadership section)
1b. **`sitemap.xml`** — official, machine-readable, and often reveals the exact
   leadership/management URL that path-guessing misses (grep it for leadership /
   management / team / executive / governance slugs).
2. **Site-scoped searches** (the owner's query patterns):
   `"<company> executive team"`, `"<company> executive committee"`,
   `"<company> management team"`, `"<company> leadership team"`,
   `"<company> board of management"`, `"<company> senior management"`
   — accept only results ON the official domain (Article III).
3. **PDF discovery** (E8 regions & govt): site-scoped search for
   `"<company> executive" filetype:pdf`, governance/annual-report pages that link
   PDFs. Annual/financial reports are LAST resort (B9) — extract the executive
   section only; do not wander the filings.
4. Nothing found on the official property → B4 fallback. Never leave the domain.

### Phase C — Extract
- From the chosen page/PDF: every listed person with their exact published title.
- **Titles are copied VERBATIM — never paraphrased or normalized.** "Executive
  General Manager, People" stays exactly that; it does not become "HR Head".
- Capture the source URL per person (page URL, or PDF URL + page/section).
- Keep original-language titles; add an English gloss in parentheses when
  confident (E6). The company's designation wins (B7).
- Founders listed as the team ARE the team (B5). Skip board/advisors sections
  unless asked.

### Phase D — Structure (single-root rule, B8)
- Root = the person the page presents as top leader (CEO / MD / Managing Officer /
  President / co-CEOs side by side per E3).
- Everyone else listed → direct child of root. NO inferred reporting lines.
- Only mirror deeper structure if the page itself explicitly draws it.

### Phase E — Render
Emit, in order (spec §4):
1. **Header:** resolved company + official domain + `As of <date accessed>`.
2. **Mermaid chart** (`graph TD`), root(s) on top, one node per person, labels
   ALWAYS quoted — `id["Name<br/><i>Title</i>"]` — so parentheses, ampersands,
   apostrophes, and non-Latin scripts can't break the diagram. For very large
   teams (E7): include everyone; the roster table is the source of truth and the
   diagram may note its own width — never truncate.
3. **Roster table:** `| # | Name | Title | Source |` — source as a markdown link.
4. **Notes:** anything honest — unverifiable people, conflicts (E5), PDF-sourced
   caveats, staleness flags (Article VII).
5. **Receipt line:** `N executives · source: <type: page|pdf> · <domain> · as of <date>`.

### Fallback template (B4) — exact shape the skill mandates
> Searched: <domain or resolved candidates> via <paths/queries tried>.
> Found: <what was actually found, or "no leadership disclosure">.
> I won't guess an org chart (wrong beats empty — not here).
> Next step: <provide the official domain | check <best-guess page> manually>.

## 3. Skill-format specifics

- Frontmatter `name: exec-company-org-chart`; `description` written for the
  agent's trigger decision: company name/account in hand + need leadership/org
  chart → fire. Mentions: no data feeding needed (the anti-CSV differentiator).
- Body ≤ ~150 lines: procedure (A–E), the rules distilled from constitution+spec
  (official-only, single root, founders in / board out, complexity fallback,
  PDF-in-scope/filings-last-resort), output templates, one worked example.
- No `metadata.internal` — public skill.

## 4. Test matrix (Gate 6 verification, maps to spec §7)

| Test | Case | Must satisfy |
|---|---|---|
| T1 | Microsoft (name only) | S1, S2 — full sourced chart, zero questions |
| T2 | costco.com.au (domain given) | S6 — AU entity, no HQ "correction" |
| T3 | Costco (name only) | §3 — main/global HQ entity |
| T4 | Tata (holding name) | S4/E4 — holding-entity chart from tata.com or honest fallback |
| T5 | Japanese co (e.g. Toyota) | B7/E6 — regional titles honored |
| T6 | PDF-disclosure org (APAC/govt entity) | S5/E8 — chart cites the PDF |
| T7 | Founder-led startup | S7/B5 — founders are the chart |
| T8 | Obscure/unresolvable name | S4/B4 — complexity fallback, zero guesses |

Executed by running the drafted SKILL.md procedure live (this session, real web)
against all 8; S2 (zero fabrications) audited by clicking every source link.

## 5. Publish steps (after tests pass)

1. Repo public on GitHub (`asheSky/exec-company-org-chart`), MIT license.
2. README with a real example output (from T1) — show, don't tell.
3. Verify `npx skills add asheSky/exec-company-org-chart` installs clean.
4. Confirm listing/discoverability on skills.sh.

## 6. Risks & mitigations

- **Leadership pages rendered by JS** (fetch returns empty shell) → skill instructs
  trying site search/alternate paths; if truly unreadable → B4 fallback.
- **Agent tool variance** (not all agents have PDF readers) → skill degrades
  honestly: if it can't read the PDF it found, it says so and links it.
- **Description too vague to trigger** → description written with concrete trigger
  phrases ("org chart", "leadership team", "who runs", "executive team").
