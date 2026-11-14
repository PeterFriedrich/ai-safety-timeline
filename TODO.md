# TODO

The source of truth for what's left. Read first every session; update in place.

**Format contract** (`tools/todo_archive.py` depends on it): top-level items are
`- [ ]` / `- [x]` lines directly under `## Open work`; `###` sub-headings may
group them; closed items are moved to `docs/TODO_archive.md` by the tool, which
leaves a one-line stub under `## Done`. An open item can be stale — reproduce the
symptom and re-measure the stated cause before acting on it.

## Open work

- [ ] **Phase 1: candidate lists.** One line each with main source, kind and proposed tier: ~50 events, 5-8 before-computers entries, ~30 terms, 6 risk categories, 10-12 forecasts. Done when the owner has cut them to the v1 set. Plan: `docs/SPEC_site.md` §8.
- [ ] **Phase 2: content fields, guards, samples.** Fields per `docs/SPEC_site.md` §5, guards per §7, 3 reviewed sample entries of each kind. Done when the guards are red on a broken entry and the owner has approved the samples' voice.
- [ ] **Phase 3: mock-up.** Timeline (past points, future ranges, risk cards) and one event page as a click-through page. Done when the owner approves the layout.
- [ ] **Phase 4: site skeleton and deploy.** Astro build plus GitHub Pages workflow (CI change: own PR, owner go-ahead). Read `docs/WEB_CACHE_BUSTING.md` first. Done when the samples are live.
- [ ] **Phase 5: draft the rest.** Batches of ~8 entries, one PR each, owner reviews. Done when the v1 set is all `reviewed`.
- [ ] **Phase 6: launch pass.** About page, phone and accessibility check, link check.
- [ ] **Open questions for the owner** (`docs/SPEC_site.md` §9): text licence (before Phase 6), review dates shown, final site name.

## Done
