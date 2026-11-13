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
| Hosting | GitHub Pages. |

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

## 5. Content model

Content lives in the repo as one Markdown file per entry, with structured
fields at the top. Five kinds:

| Kind | Key fields |
|---|---|
| `event` | date (and precision: day, month or year), title, themes, summary, body, terms used, deeper links, sources, review status |
| `term` | name, short definition, body, related terms, deeper links, sources, review status |
| `risk` | name, the claim, arguments for, arguments against, related forecasts, sources, review status |
| `forecast` | who, date published, what is predicted (quoted), range start, range end, best guess if any, method, criticism, source, date last checked, review status |
| `source` | title, author or organisation, date, URL, archived URL, type (primary, journalism, explainer) |

Review status is `draft` or `reviewed`, with the review date. **Only reviewed
entries are built into the site.**

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
| After | Weekly link check, 6-monthly forecast re-check, new events as they happen. | GitHub issues. |

## 9. Still open

1. **Address:** the default GitHub Pages address, or a custom domain?
2. **Name:** what the site is called on the page.
3. **Start date:** 1950 (Turing's paper) or earlier?
4. **Licence for the text:** Creative Commons Attribution, or all rights
   reserved?
5. **Byline:** is the owner named on the About page?
6. **Review dates:** shown to readers on each entry, or kept internal?
