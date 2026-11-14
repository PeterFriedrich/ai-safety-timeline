# SPEC — the site plan

The plan for the AI safety timeline site: what gets built, in what order, and
the rules the content follows. Planning only; no code yet. Read before starting
any phase below. Locked choices have a row in `docs/DECISIONS.md`; anything
under §9 is still open.

## 1. What it is

A static website for a lay reader, hosted on GitHub Pages. One timeline with
two halves:

- **Past:** events in AI development and AI safety, as points on a date axis.
- **Future:** possible threats, as overlapping date ranges that other people
  have published, plus undated risk categories.

Every event and key term gets a short plain-language explanation (the first
layer) and links to deeper resources. The site explains and points to sources.
It publishes no forecasts or probabilities of its own.

## 2. Owner's answers (2026-11-13)

| Question | Answer |
|---|---|
| v1 size | About 30 to 40 curated events from the 1950s to today, about 20 to 30 terms. |
| Future half | Both: undated risk categories, and attributed forecasts drawn as overlapping date ranges. The ranges are the main visual. |
| Authorship | Claude drafts from cited sources; nothing publishes until the owner marks it reviewed. |
| Stance | Explain the range of views, skeptics included. The site takes no side on how likely or how soon. |
| Hosting | GitHub Pages, default address `peterfriedrich.github.io/ai-safety-timeline` for now. |
| Name | Keep the working title for now. |
| Start | The main timeline starts at 1950. Earlier speculation goes in a separate "before computers" strip (§4a). |
| Byline | The owner is named briefly at the bottom of the About page. |
| Event weight | Events are marked by kind and ranked in tiers, so an iconic paper ranks with a real incident (§4b). |

## 3. Pages

| Page | What it holds |
|---|---|
| Home (the timeline) | Past events on a date axis grouped by era, a "today" line, then the future half. Filter by theme. Each item opens its own page. |
| Event page, one per event | What happened (about 150 words), why it matters for safety, the terms it uses (linked), 2 to 4 "go deeper" links, sources. |
| Glossary, plus one page per term | Plain definition (about 100 words), an example, the events and risks that use the term, deeper links. |
| Risk page, one per category | What the worry is, who argues it, who disputes it and why, the forecasts that bear on it. |
| Forecast page, one per range | Who published it, when, what exactly they predicted, the range, how they got it, and published criticism of it. |
| About | How events are chosen, sourced and reviewed; that Claude drafts and the owner reviews; that the site is not a forecast. |

## 4. The future half

The hard design problem, so it gets its own section.

- **Each forecast is a horizontal bar** from its earliest to its latest date,
  in its own lane, so bars overlap visibly where sources agree and spread where
  they don't. A bar with a stated median or best guess marks it.
- **Every bar carries its attribution on the bar itself:** who, and the year
  they published it ("Survey of AI researchers, 2023"). A reader must never
  take a bar for the site's own estimate.
- **Bars group by what is predicted**, since forecasts of different things
  can't be compared: for example "AI that can do most human work" in one group,
  "a specific harm" in another. The grouping question is each source's own
  definition, quoted.
- **The future half looks different from the past:** points are solid, ranges
  are translucent, and the "today" line separates them.
- **Risk categories carry no dates.** They sit beside the ranges as cards
  (for example misuse, misalignment, loss of control, concentration of power,
  gradual harms), each linking to its risk page.
- **Forecasts go stale.** Each one stores the date it was published and the
  date we last checked it. A range whose start date has passed stays on the
  page, marked as such, with what happened.

Draw this as a mock-up before any build work (Phase 3).

## 4a. Before computers

A short, separate strip before 1950 for interest: people who imagined
thinking machines, or machines escaping control, before any existed. Examples:
Ada Lovelace's notes (1843), Samuel Butler's "Darwin among the Machines"
(1863), Karel Čapek's play *R.U.R.* (1920), which coined "robot".

- Collapsed by default and labelled as speculation and fiction, not events.
- Does not count toward the 30 to 40 v1 events. About 5 to 8 entries.
- Same entry format and review rule as events.

## 4b. Event kinds and tiers

Two separate markings on every event.

**Kind** (what it is), shown by icon and colour:

| Kind | Examples |
|---|---|
| Incident | A system causing real harm or behaving in a way nobody intended |
| Research | Papers, technical results |
| Writing | Books, essays, blog posts, open letters |
| Capability | A system that changed what people thought was possible |
| Policy | Laws, executive orders, international agreements |
| Institution | A lab, safety organisation or government body founded |

**Tier** (how much it matters), shown by size and by when it appears:

| Tier | Meaning | On the timeline |
|---|---|---|
| 1, landmark | The story can't be told without it. An iconic paper sits here beside major incidents. | Always shown, large, with its title. |
| 2, notable | Important, but the reader can follow the story without it. | Shown smaller, title on hover or tap; a filter can hide it. |
| 3, context | Background that enriches the story: minor papers, posts, smaller events. | Hidden by default; a "show all" filter adds it as small dots. |

Tier and kind are independent: a paper can be tier 1 and an incident tier 2.
Each event records one sentence on why it got its tier, so tiering stays
consistent across batches. Roughly 15 tier 1 and 20 tier 2 in v1, and tier 3 as it comes up (outside the 30 to 40 count); the
Phase 1 lists propose a tier per event for the owner to change.

## 5. Content model

Content lives in the repo as one Markdown file per entry, with structured
fields at the top. Five kinds:

| Kind | Key fields |
|---|---|
| `event` | date (and precision: day, month or year), title, kind, tier, reason for tier, themes, summary, body, terms used, deeper links, sources, review status |
| `term` | name, short definition, body, related terms, deeper links, sources, review status |
| `risk` | name, the claim, arguments for, arguments against, related forecasts, sources, review status |
| `forecast` | who, date published, what is predicted (quoted), range start, range end, best guess if any, method, criticism, source, date last checked, review status |
| `source` | title, author or organisation, date, URL, archived URL, type (primary, journalism, explainer) |

Review status is `draft` or `reviewed`, with the review date. **Only reviewed
entries are built into the site.**

**Dates shown to readers.** Each entry shows "Human checked: <month year>",
the date the owner last reviewed it. Later, when some updates are automated
(§8, After), an entry changed by automation after its last human check also
shows "Updated automatically: <date>, not yet checked by a person" until the
owner reviews it again. v1 only needs the human-checked date.

## 6. Editorial rules

- **Inclusion:** an event is in if a later event or a current debate can't be
  explained without it. Product launches are in only when they changed what
  people thought was possible.
- **Sources:** the primary source first (the paper, the statement, the law),
  then one good explainer. Every factual claim in an entry traces to a listed
  source.
- **Contested claims:** state what each side claims and link to its best
  version. Words like "will" and "likely" appear only inside an attributed
  claim.
- **Reading level:** written for someone with no technical background. A term
  is either explained in place or linked to the glossary on first use.
- **Length:** the first layer stays short; depth belongs to the links.

## 7. How it is built

- **Astro**, the same tool as the owner's portfolio site, also on GitHub
  Pages. It builds static pages from the content files and rejects an entry
  with missing or wrongly typed fields at build time.
- **The timeline is plain HTML, CSS and SVG** with a little JavaScript for
  filtering. The page is readable with JavaScript off, on a phone, and by a
  screen reader (the timeline is also a plain ordered list underneath).
- **Deploy:** a GitHub Actions workflow builds and publishes on merge to
  `master`. Default address `peterfriedrich.github.io/ai-safety-timeline`.
- **Rejected:** hand-written HTML (80 or more pages with no field checks); an
  off-the-shelf timeline widget (none draws overlapping attributed ranges
  well, and they don't work without JavaScript).
- **Fit with the template:** the template's guards are Python and CI already
  runs them, so content guards stay in Python and read the same content files.
  Adding the Astro build and the deploy workflow is a CI change and needs the
  owner's go-ahead in its own PR.

### Guards, written before the content they protect

| Guard | Checks |
|---|---|
| Review gate | No entry with status `draft` appears in the built site. |
| Sourcing | Every event, term, risk and forecast has at least one source; every event and term has at least one deeper link. |
| Attribution | Every forecast has who, date published, quoted prediction, and both ends of the range. |
| Cross-links | Every term, risk, forecast and source an entry refers to exists. |
| Links | Every outside URL answers; runs weekly and files a GitHub issue on failure. |
| Length | Each first-layer text is under its word limit. |
| Stale forecasts | A forecast not re-checked in 6 months is listed in a GitHub issue. |

## 8. Phases

Each phase ends at something the owner looks at.

| Phase | Work | Owner sees |
|---|---|---|
| 0 | This plan and its decision rows. | This PR. |
| 1 | Candidate lists, one line each: about 50 events, 30 terms, 6 risk categories, 10 to 12 forecasts, each with its main source. | The lists, to cut and add to. |
| 2 | Content fields, the guards above, and 3 finished sample entries of each kind. | The samples, to settle voice and length. |
| 3 | Mock-up of the timeline (both halves) and one event page, as a page to click through. | The mock-up. |
| 4 | Site skeleton, deploy workflow, the sample entries live. | The live site with samples. |
| 5 | Remaining entries drafted in batches of about 8; owner reviews each batch. | Each batch as a PR. |
| 6 | About page, phone and accessibility pass, link check, launch. | The finished v1. |
| After | Weekly link check, 6-monthly forecast re-check, new events as they happen. Later: some automated updates (scope not set), shown as unchecked until reviewed (§5). | GitHub issues. |

## 9. Still open

1. **Licence for the text:** Creative Commons Attribution, or all rights
   reserved? Owner will decide later; needed before launch (Phase 6).
2. **Final site name:** working title until then.
3. **Automated updates:** what gets updated automatically, and from where. Not
   needed for v1.
