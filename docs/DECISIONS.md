# Decisions Index

Append-only. **One ROW per locked decision** — when, what, why (including what
was rejected), and a pointer to where the argument lives in full. When a decision
locks, add a row; when one is superseded, strike it (`~~...~~`) or mark it
`SUPERSEDED <date>` in place and add the successor — don't delete history.

**What a row owes you:**

1. ⚠️ **EVERY ROW CARRIES A POINTER TO A DOC** — not only to code. Code moves;
   the argument has to live somewhere prose can hold it.
   `scripts/check_doc_citations.py` checks that every pointer resolves.
2. **The row is a self-contained summary** and may paraphrase the argument.
3. **The pointer is the authority.** When a row and its target disagree, the
   target wins and the row gets fixed.
4. **A row names the test that protects it**, or carries `[unverifiable]`.
   `scripts/check_decisions_log.py` enforces this on the merge gate (and that a
   superseded row is marked where it stands).

| When | Decision | Full reasoning |
|------|----------|----------------|
| 2026-11-13 | **v1 is about 30-40 curated events (1950s to today) and 20-30 terms.** Rejected: a 10-15 event pilot, and 100+ events before anything ships. [unverifiable] | `docs/SPEC_site.md` §2 |
| 2026-11-13 | **The future half shows both undated risk categories and other people's forecasts as overlapping date ranges; the ranges are the main visual, each attributed on the bar.** The site publishes no forecast of its own. [unverifiable] until the attribution guard exists. | `docs/SPEC_site.md` §4 |
| 2026-11-13 | **Claude drafts entries from cited sources; only entries the owner marks reviewed are published.** Rejected: owner writes everything; publish on automated checks with spot review. [unverifiable] until the review-gate guard exists. | `docs/SPEC_site.md` §5, §7 |
| 2026-11-13 | **Contested claims: explain the range of views, skeptics included; the site takes no side on likelihood or timing.** Rejected: a safety-concerned voice; events with no debates. [unverifiable] | `docs/SPEC_site.md` §6 |
| 2026-11-13 | **Hosted on GitHub Pages as a static Astro site.** Rejected: hand-written HTML, an off-the-shelf timeline widget. [unverifiable] | `docs/SPEC_site.md` §7 |
| 2026-11-14 | **Events carry a kind (incident, research, writing, capability, policy, institution) and a tier (1 landmark, 2 notable), set independently, so an iconic paper ranks with a major incident.** [unverifiable] until the content-fields guard exists. | `docs/SPEC_site.md` §4b |
| 2026-11-14 | **The main timeline starts at 1950; earlier speculation is a separate, collapsed "before computers" strip outside the v1 event count.** [unverifiable] | `docs/SPEC_site.md` §4a |
