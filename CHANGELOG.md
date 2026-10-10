# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project uses [Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-10-09

### Added

- Initial public release of the Mobile Agent Dashboard template edition.
- Single self-contained `index.html` — no build step, no dependencies,
  works offline by opening the file.
- Profile view: operator card with the station logo, total level
  medallion, and an HP-style money-goal bar with an editable goal.
- Skills view: 21 business/operator skills in four schools, on a classic
  exponential XP curve (levels 1–99, max total level 2,079). Click-through
  detail panel with full XP math and per-skill task history.
- Quest Book: quest lines with status, objective, checkable steps,
  reward, and deadline — seeded with clearly-marked demo quests.
- Inventory: 28-slot grid with click-through item details and add/remove.
- Agent Ops station floor: 12 zones under the visible law
  "Truthful telemetry — the station only shows work the system can prove."
- Task Log: add entries with standardized 10 / 25 / 50 / 100 XP awards.
  Skill XP is computed from the log, so every level is auditable.
- Weekly view: XP this week (Monday–Sunday, local time), per-day chart,
  last-week comparison, and XP by skill.
- Persistence via `localStorage`, with a one-click reset to demo state.
- Strict green / red / white palette on dark neutrals, themeable via CSS
  variables; original "A in a split reactor ring" logo in the header,
  on the profile card, and as `assets/logo.svg`.
