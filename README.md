# skills

Skills for everyday people who use AI agents to get things done.

These are [Agent Skills](https://skills.sh) — portable instruction bundles that teach an AI agent how to accomplish a task. Each skill describes *what* to do in capability terms, so an agent can load whatever tools it needs and follow along.

## Install

Install all skills from this repo:

```
npx skills add openaidom/skills
```

Or install a single skill:

```
npx skills add openaidom/skills --skill lost-or-stolen-item-finder
```

## Available skills

| Skill | What it does |
|-------|--------------|
| [lost-or-stolen-item-finder](skills/lost-or-stolen-item-finder/SKILL.md) | Locate a lost or stolen item listed for resale on second-hand marketplaces, maintaining a ranked leaderboard of candidates across sessions. |
| [flight-deal-monitoring](skills/flight-deal-monitoring/SKILL.md) | Continuously search, compare, and monitor flight prices, alerting on fare drops and recommending when to book. |
| [raw-text-to-bitwarden-csv-converter](skills/raw-text-to-bitwarden-csv-converter/SKILL.md) | Convert raw text containing credentials into the CSV format required for importing into Bitwarden. |

## License

[MIT](LICENSE)
