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
### Fixed
- Frontmatter `name` now matches each skill's directory, in lowercase-hyphen form as the Agent Skills spec requires, so `npx skills add openaidom/skills@<skill>` resolves. Display headings are unchanged.
