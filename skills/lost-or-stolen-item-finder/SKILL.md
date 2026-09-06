---
name: Lost or Stolen Item Finder
description: >-
  Locate a lost or stolen personal item that may have been listed for resale on
  second-hand marketplaces. Runs as a long-running, resumable operation that
  builds and maintains a ranked leaderboard of candidate listings across many
  runs, so a human can check the best matches against the real item. Use
  when someone wants to search resale marketplaces for a specific missing item.
allowed-tools: Bash
---

# Lost or Stolen Item Finder

You can help locate a lost or stolen personal item that may have been listed for resale on second-hand marketplaces. This is a long-running, resumable operation: you make progress in bounded batches ("runs"), and each run resumes from state saved by the previous one. Across runs you progressively search the web and relevant marketplaces, and maintain a ranked leaderboard of candidate listings, so a human can check the best matches against the real item.

This work surfaces candidates and evidence. It does not arrange purchases or contact sellers. When a strong candidate is found, the intended next step is reporting to the police and the platform.

## Capabilities you may need

This skill describes *what* to do, not which tools to use. Depending on your environment, you may need to **find a skill, use a tool you already have, or write a custom script (e.g. shell/`curl`)** to:

1. **Search the web** for listings and search-result pages across marketplaces.
2. **Browse or crawl pages** that resist simple fetching (marketplaces with heavy bot protection).
3. **Save and re-read your own working notes** (files or equivalent).
4. **Remember a small amount of state durably across runs**, in case your notes do not survive between runs.
5. **Schedule a follow-up run.** If you cannot schedule one yourself, ask the user to re-run you.
6. **Message the user and publish results.**

Prefer the smallest set of capabilities that gets the job done.

## Item details

The following are required. If the user has not provided them, ask.

- item-type =
- brand =
- defining-attributes = (size, colour, model, features)
- last-seen-location =
- last-seen-date =

Optional:

- starting-marketplace = (default: the largest regional second-hand marketplace)
- report-frequency = 6 hours
- distinguishing-marks = (scratches, stickers, serial number — what makes THIS item unique)

## Continuity — single entry point

On startup, look for an entry-point document `AGENTS.md` among your saved notes. If it is missing, you have not created it yet — create it, then begin. `AGENTS.md` must link to every other state document (to any depth of nesting) so future-you never has to guess the structure. Suggested documents, all reachable from `AGENTS.md`:

- `AGENTS.md` — mission, current status, last-run summary, next-run plan, links to everything below.
- `ranking-rubric.md` — scoring criteria and weights.
- `leaderboard.md` — ranked candidates with scores and per-criterion breakdown.
- `seen-ledger.md` — every listing already evaluated, keyed by listing ID.
- `search-log.md` — every search vector attempted, with timestamp and outcome.

Saved notes may not survive between runs, so also keep a durable pointer and a compact leaderboard snapshot in long-term memory each run, and recover from it if the notes are gone.

## Each run

1. Load state: read `AGENTS.md` and follow its links; fall back to long-term memory if the notes are missing.
2. Choose new search vectors from `search-log.md` — do not repeat a vector already logged.
3. Run a bounded batch of searches (a handful per run), searching and crawling the web and marketplaces.
4. For each new hit, capture evidence: listing ID, URL, seller, location, price, post date, image URLs, capture timestamp. De-dupe by listing ID, not URL.
5. Score each hit with the rubric, update the leaderboard and the memory snapshot, extend the seen-ledger and search log.
6. Persist state to your notes and to long-term memory, then schedule the next run (or ask the user to re-run you).

## Ranking rubric (pre-committed)

Keep the rubric in `ranking-rubric.md` and link it from `AGENTS.md` so it is obvious at the start of every run. Do not re-invent it per run; if you change it, version it and note why. Score every hit 0–100 as a weighted sum and record the per-criterion breakdown so a human can audit the ranking. A starting rubric (tune to the item and record changes):

- Brand/model match — highest weight, especially for rare or distinctive brands.
- Location proximity to the last-seen location — high weight; stolen items usually resurface nearby.
- Listing recency (posted after the last-seen date) — meaningful weight.
- Each matching physical attribute (size, colour, features) — moderate weight each.
- Price plausibility — small weight; flag suspiciously cheap or "quick cash sale" listings.

Flag any candidate above a threshold you choose as "verify with owner".

## Bot protection

Marketplaces use aggressive bot protection. Even browsing as a real user would, you may hit CAPTCHAs. Treat blocks as a normal state: log "blocked/unreachable" against that vector in the search log and move on rather than hammering it. Respect each site's terms.

## Reporting

Every `report-frequency` (or once per N runs if runs are sparse), send the user a review that summarises the whole operation, presents the current leaderboard top-N with scores and reasons, and recommends whether to extend, continue, or wind down — and by how long. Continue until the user stops the task.
