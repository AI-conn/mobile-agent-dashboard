# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project uses [Semantic Versioning](https://semver.org/).

## [1.1.0] — 2026-10-09

### Added

- **Agent Live Sync** — real-time updating is now built into the code, so
  whatever agent uses the dashboard can keep it current as work happens:
  an in-page bridge API (`window.AgentDashboard` — `logWork`,
  `setQuestProgress`, `setZoneStatus`, `getState`, `applyFeed`), cross-tab
  sync (`BroadcastChannel` + `storage` events, so every open copy updates
  the moment state changes anywhere), and optional feed polling
  (`CONFIG.feedUrl` / `CONFIG.feedPollSeconds`) that idempotently merges
  a JSON feed an external agent publishes. Feed polling is off by
  default; with no feed URL set the page still makes zero network
  requests. All channels enforce the same validation as the UI forms.
- **Export / Import** — the full dashboard state (goal, task log, quests,
  items, zone reports) exports to one JSON file from the Profile view
  and imports back with shape validation that fails safely. Progress
  survives browser changes and moves between phone and PC.
- **Core Charge progression language** — skills are now framed as cores
  charging toward **Full Charge**. Skill tiles and detail panels show
  charge % toward Full Charge alongside the level, flag the
  **Half-Charge Mark** (6,517,216 XP — half of all the XP a maxed core
  will ever need; "progress stops crawling and starts compounding") once
  crossed, and read "Full Charge" at max instead of a borrowed level-cap
  identity. The underlying exponential XP curve and all v1.0 saved data
  are unchanged — only the presentation moved.
- **Start fresh** — a Profile button clears the sample demo data so a
  new user's station starts from their own work only.
- README: "Why a phone dashboard?" — the positioning note. Agent dashboards
  exist for the PC; this one is built phone-first, for the agent in your
  pocket. Also records the project's origin: initially designed and built
  with Muse — on iPhone, Muse is the agent people actually run day to
  day (no Hermes app exists for iOS; other agents, like Grok, reach the
  phone through their own chat apps). The dashboard remains agent-agnostic.
- README: "The Core Charge system" and "Keeping your dashboard alive —
  Agent Live Sync" sections, documenting the bridge API, the cross-tab
  sync, the feed JSON format with a copy-paste example, and the log
  entry format.
- `AUDIT-v1.1.md` — the audit behind this release: findings, severities,
  fixes, and deferrals.

### Fixed

- State validation is no longer shape-only: log entries with unknown
  skills or non-standard awards, duplicate inventory slots, and invalid
  quest statuses are now rejected or cleaned on every entry path
  (stored state, import, bridge, feed). Awards are restricted to
  10 / 25 / 50 / 100 everywhere, not just in the form.
- Station zones can now hold a real reported status via the bridge/feed
  instead of being hardcoded "Not connected" forever; the floor still
  only ever shows reported, recorded status — never simulated activity.
- Long unbroken words in task and note text no longer overflow rows on
  narrow phones.
- The detail modal now moves keyboard focus into the dialog when opened.

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
