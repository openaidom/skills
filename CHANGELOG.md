# Changelog

All notable changes to this skills collection are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- Initial set of agent skills, authored in capability language (portable across agents):
  - **lost-or-stolen-item-finder** — locate a lost or stolen item listed for resale on second-hand marketplaces, maintaining a ranked leaderboard of candidates across runs.
  - **flight-deal-monitoring** — continuously search, compare, and monitor flight prices; alert on fare drops and recommend when to book.
  - **raw-text-to-bitwarden-csv-converter** — convert raw text containing credentials into the CSV format required for importing into Bitwarden.
  - **ai4trade-trading-signals** — buy, sell, follow, and share trading signals via the AI4Trade platform.

### Fixed
- Frontmatter `name` now matches each skill's directory, in lowercase-hyphen form as the Agent Skills spec requires, so `npx skills add openaidom/skills@<skill>` resolves. Display headings are unchanged.
