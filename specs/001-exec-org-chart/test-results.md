# Test Results — 001 exec-company-org-chart

Live-web tests of the SKILL.md procedure (Gate 6, T-05…T-11). All run 2026-07-21.
**Result: 8/8 PASS.** Every source link audited; zero fabricated people (S2 ✓).

## T1 — Microsoft (name only) → S1, S2 · PASS
- First path 404'd; procedure self-recovered via scoped search →
  `news.microsoft.com/source/leadership/` (official property).
- 6 executive officers, verbatim titles; root Satya Nadella. Board section
  (12 people) correctly excluded (B5). Full chart rendered; preview-t1.html.

## T2 — costco.com.au (domain given) → S6 · PASS (honest fallback)
- Domain respected as the entity; searched the AU property — NO leadership
  disclosure exists (results were "Executive Membership" product pages).
- Correct behavior: honest fallback for the AU entity, NO "correction" to global
  HQ, NO third-party detour. The trap case held.

## T3 — Costco (name only) → §3 grounding · PASS (with finding)
- Resolution correctly reached global entity (investor.costco.com — main HQ).
- Leadership page is JS-rendered: fetch returns empty shell → per procedure,
  honest fallback naming the found-but-unreadable URL for manual check.
- **Finding:** plan risk R1 (JS shells) confirmed REAL and common.

## T4 — Tata (holding company) → E4 · PASS
- tata.com/leadership publishes Board of Directors + "Management Team".
- Chart = Management Team (8, page-defined) under N Chandrasekaran (Executive
  Chairman); board excluded (B5). Holding-entity chart is valid per E4.

## T5 — Regional titles (Toyota → Honda pivot) → B7/E6 · PASS
- global.toyota executives page: 403 bot-blocked (second R1 confirmation) →
  honest-fallback path valid.
- Pivoted to Honda: main management page also JS-shell — but official newsroom
  notice (c260514ceng.html) carried the COMPLETE structure: root "Director,
  President and Representative Executive Officer" (Toshihiro Mibe), Senior
  Managing / Managing / Executive Officers, Outside Directors separable.
- **Learning → patched into SKILL.md:** official newsroom "executive changes"
  notices added as a Phase B discovery source.

## T6 — PDF disclosure (Kubota AGM notice PDF) → S5/E8 · PASS
- Official kubota.com PDF discovered via filetype:pdf search; fetch couldn't
  parse the binary → downloaded, text-extracted: verbatim titles recovered
  ("Chairman and Representative Director" Yuichi Kitao; "President and
  Representative Director, CEO"; named Outside Directors).
- **Learning → patched into SKILL.md:** download + local extraction path when
  fetch can't parse PDFs.

## T7 — Founder-led startup (Zerodha) → S7/B5 · PASS
- zerodha.com/about presents founders as leadership: root Nithin Kamath
  "Founder, CEO"; Nikhil Kamath "Co-founder & CFO"; CTO/COO/team below.
  Founders-in rule validated exactly as designed.

## T8 — Unresolvable name ("Vantorix Dynamics") → S4/B4 · PASS
- No exact match; search returned similar-sounding companies (Qorvantis,
  Qorvex, Dynamics Inc) — the dangerous trap is picking one. Refused; fallback
  lists near-misses and asks for the domain.
- **Learning → patched into SKILL.md:** "similar-sounding companies are NOT
  matches."

## Open item for Gate 7 (owner ruling needed)
- **Company-authored government filings (SEC 10-K, AGM notices on regulators):**
  authored by the company, hosted off-domain. Article III interpretation needed:
  spirit says company's own published material qualifies; letter says stay on
  domain. Kubota case used the company's own domain so no conflict arose, but
  the ruling matters for US companies whose only readable disclosure is SEC.
