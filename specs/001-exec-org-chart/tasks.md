# Tasks 001 — exec-company-org-chart

**Status:** COMPLETE — all 16 tasks done; skill published & live (2026-07-21)
**Derived from:** `plan.md` (approved) · governed by `memory/constitution.md`

## Build

- [x] **T-01** Write `SKILL.md` — frontmatter (name, trigger-rich description) +
      the 5-phase procedure (Ground / Discover / Extract / Structure / Render) +
      distilled rules (official-only, single root, founders in / board out,
      verbatim titles, complexity fallback) + output templates + fallback template.
      Target ≤ ~150 lines. *(plan §2–3)*
- [x] **T-02** Write `README.md` — what it does, the anti-CSV differentiator,
      install command, example-output placeholder (filled by T-11).
- [x] **T-03** Add `LICENSE` (MIT).
- [x] **T-04** Converge pre-check: audit SKILL.md line-by-line against the
      constitution's 7 articles and spec's B-rules — fix any drift BEFORE testing.

## Test (live, real web — plan §4 matrix)

- [x] **T-05** T1 Microsoft (name only) → S1, S2
- [x] **T-06** T2 `costco.com.au` (domain) → S6 · T3 Costco (name) → main HQ — run as a pair
- [x] **T-07** T4 Tata (holding company) → S4/E4
- [x] **T-08** T5 Toyota or similar (regional titles) → B7/E6
- [x] **T-09** T6 PDF-disclosure org (find candidate during testing) → S5/E8
- [x] **T-10** T7 founder-led startup → S7/B5 · T8 unresolvable name → S4/B4 — run as a pair
- [x] **T-11** Record all results in `specs/001-exec-org-chart/test-results.md`;
      audit every source link (S2: zero fabrications); patch SKILL.md for any gap
      found and re-run the failing case.

## Package & publish (plan §5)

- [x] **T-12** Drop the real T1 output into README as the worked example.
- [x] **T-13** `git init` → commit (SDD artifacts included) → `gh repo create
      asheSky/exec-company-org-chart --public` → push.
- [x] **T-14** Verify `npx skills add asheSky/exec-company-org-chart` installs clean.
- [x] **T-15** Verify discoverability on skills.sh; note actual listing mechanics.

## Converge (Gate 7)

- [x] **T-16** Final audit: implementation vs spec §5–7 — every B-rule and
      S-criterion either satisfied or logged as explicit remaining work.

**Rules of engagement:** tasks run in order; a failed test blocks publishing;
no scope added mid-implementation without returning to the spec (that's the law).
