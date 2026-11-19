# Changelog

All notable changes to this skills collection are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- job-search

- Initial set of agent skills, authored in capability language (portable across agents):
  - **lost-or-stolen-item-finder** — locate a lost or stolen item listed for resale on second-hand marketplaces, maintaining a ranked leaderboard of candidates across runs.
  - **flight-deal-monitoring** — continuously search, compare, and monitor flight prices; alert on fare drops and recommend when to book.
  - **raw-text-to-bitwarden-csv-converter** — convert raw text containing credentials into the CSV format required for importing into Bitwarden.

### Changed
- job-search: continuity between runs (run budget, run state, checkpoints, locks, scheduler setup, restore/close steps) is removed; the skill now lists task continuity as a required capability. File paths are now storage keys, and durable storage may be a file, database, cache or other store. The job database is one record per job (`jobs/<job-id>`). The assignment no longer has an expiry: it is active until the user asks to stop, and searches and reports run only while it is active. The search keeps its own progress: a cursor per source/query in the ledger (restarted with an overlapping date window when it expires), a page cap per source per pass, and new listings saved as `unchecked` (separate from `unverified`) and checked first. Reports are built from a compact `jobs/summary` record instead of reading every job record. The skill states when its next work is due (the earliest of the next due search, the next daily report time and the next retry time), and report state keeps delivery retry times again. The skill keeps its own records, cadence, reports and final report at assignment end.
- job-search: a search is now a sweep over a planned frontier of sources, queries and validations, and a run ends when a newly planned frontier is empty, it is blocked or its time allowance is used up, not after one pass. Validation never blocks discovery: listings the user cannot take or that fail a stated requirement are dropped with a reason (ranking criteria rank, they do not exclude), and the rest are validated in batches. A source list (`sources`) records each source's type, access, health, backoff and yield, and `references/discovering-sources.md` explains how to find and vet new sources for the user's own country, languages and field, including employer career pages and ATS feeds; a table in `references/finding-and-validating-listings.md` gives the board feed and open-check address of common ATS platforms, as hints to test. The ledger is one keyed entry per source, query or result page, written as `planned` before the request, with a reason code on every outcome other than `ok`. The skill no longer assumes a corporate or tech market: sources are searched where this kind of job is advertised in the user's country and field (including agencies, staffing firms and public employment services), channels that cannot be read are reported as coverage gaps, the ATS endpoints are an optional list, and eligibility covers location, work authorisation, language, licences and schedule. The ranking options add schedule, commute and relocation limits, more employment types and any other criteria. All storage keys now start with `openaidom/job-search/`, and durable storage must survive a restart of the agent.

### Fixed
- Frontmatter `name` now matches each skill's directory, in lowercase-hyphen form as the Agent Skills spec requires, so `npx skills add openaidom/skills@<skill>` resolves. Display headings are unchanged.
