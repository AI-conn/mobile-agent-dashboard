# Mobile Agent Dashboard

A gamified operator dashboard that treats your work like a character sheet:
**21 skills** that level on a classic exponential XP curve, a **quest book**, a
**28-slot inventory**, an **agent-ops station floor**, and a **task log** that
is the proof behind every level. Nothing levels up on claims — XP is earned
only from logged, completed work.

This is the **template edition**: a single self-contained `index.html` with
demo data, ready to fork and make your own. No build step, no dependencies,
no accounts, no tracking. All data lives in your browser's `localStorage`.

## Works with any agent

This dashboard is agent-agnostic by design. Use it with a hosted agent, a
local agent framework, a custom script, or no agent at all. Plug in your own
setup by editing `CONFIG` (operator and station names, plus the goal) and
the `ZONES` list in `index.html` so the station floor matches how you work.

It stays a standalone static page: demo data only, no accounts, no tracking,
and no phoning home. Work you enter stays in your browser's `localStorage`.

![Logo](assets/logo.svg)

## Features

- **Profile** — operator card with logo, total level medallion, HP-style
  money-goal bar with an editable goal (name, target, earned).
- **Skills** — 21 business/operator skills in four schools. Click any skill
  for the full detail panel: total XP, the exact XP math for the current and
  next level, and the complete task history behind the level.
- **Quest Book** — one quest line per project, with status, objective,
  checkable steps, reward, and deadline.
- **Inventory** — a 28-slot grid for project pieces you want visible: drafts,
  files, decisions, leads, key dates, assets. Click a slot for full details.
- **Agent Ops (Station)** — a 12-zone station floor (Command Deck, Research
  Bay, Builder Workshop, Browser Dock, Security Vault, Outreach War Room,
  Media Studio, Data Foundry, Agent Dock, Learning Bridge, Treasury, Outbox) under
  one visible law: *Truthful telemetry — the station only shows work the
  system can prove.*
- **Task Log** — the proof ledger. Add an entry, pick a skill, award
  10 / 25 / 50 / 100 XP. Skill XP is *computed from the log*, so every level
  can be audited back to its entries.
- **Weekly** — XP earned this week (Monday–Sunday, your local time), a
  per-day chart, last week's total, and XP by skill.
- **Persistence** — everything saves automatically to `localStorage`
  under the key `mobile-agent-dashboard-template-v1`.
  A reset button restores the demo state.

## Quick start

Option 1 — just open the file:

```text
Open index.html in any modern browser. That's it.
```

Option 2 — serve it statically:

```bash
git clone https://github.com/<your-username>/mobile-agent-dashboard.git
cd mobile-agent-dashboard
python3 -m http.server 8000
# then visit http://localhost:8000
```

Option 3 — GitHub Pages: fork the repo, then go to
**Settings → Pages → Build and deployment → Deploy from a branch** and pick
your default branch with the `/ (root)` folder. Your station will be live at
`https://<your-username>.github.io/mobile-agent-dashboard/`.

## Customization

### The CONFIG object

Near the top of the `<script>` block in `index.html` there is a single
`CONFIG` object. Edit it to make the station yours:

```js
const CONFIG = {
  operatorName: "Operator",            // your operator / persona name
  operatorTitle: "Business & Finance Agent",
  stationName: "Mobile Agent Dashboard",
  goalName: "First Revenue Goal",      // default money goal
  goalTarget: 1000,                    // default goal target
  currency: "$"
};
```

The money goal can also be edited in the app itself (Profile → Update goal);
in-app edits are saved to `localStorage` and override the defaults.

### Theme tokens

Colors are CSS variables at the top of the `<style>` block (`:root`). The
palette is deliberately strict — **green, red, and white on dark neutrals**:

| Token | Role |
|---|---|
| `--bg` | App background |
| `--panel` | Card / panel background |
| `--line` | Borders |
| `--text` | Primary text (white) |
| `--muted` | Secondary text |
| `--green` | Primary accent — progress, active, healthy |
| `--red` | Highlight accent — selected, alerts, low states |

Change the tokens and the whole station follows. The logo
(`assets/logo.svg`, also inlined in the header) uses the same palette.

## How XP works

Levels run **1–99** per skill, on a classic exponential curve. The total XP
required for level *n* is:

```text
xp(n) = floor( ( Σ for k = 1..n-1 of floor(k + 300 · 2^(k/7)) ) / 4 )
```

Landmarks: level 2 = 83 XP · level 92 = 6,517,253 XP (halfway) ·
level 99 = 13,034,431 XP. Max total level across 21 skills: **2,079**.

House rules baked into the template:

- Awards are standardized: **10 / 25 / 50 / 100 XP** per logged task.
- No XP for drafts, plans, or claims — only completed, logged work.
- Skill XP is the sum of that skill's task-log entries. Delete an entry and
  the XP goes with it; corrections are never silent.
- Levels rise gradually and together. It's a marathon scale on purpose.

## Screenshots

<!-- TODO: add screenshots of your fork here.
     Suggested shots: Profile, Skills detail panel, Station floor. -->

_Screenshots coming soon — fork it, theme it, show yours._

## Project layout

```text
mobile-agent-dashboard/
├── index.html          # the entire app (HTML + CSS + JS, self-contained)
├── assets/
│   └── logo.svg        # the station logo (also inlined in index.html)
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CHANGELOG.md
└── LICENSE
```

## Reviews

This package was reviewed before its first public release (2026-10-09).
Both reviews are a required step before any public release.

- **Security Team review — ✅ PASS, clear to publish.** Full read of every
  file plus automated sweeps. Findings: no secrets, credentials, or keys
  anywhere; no personal data anywhere except the copyright line in
  `LICENSE`; zero external network requests (no fetches, no external
  scripts, styles, fonts, or images, no tracking or analytics); no
  dangerous code patterns (`eval`, `document.write`, and similar are
  absent); every place stored data is rendered passes through an escaping
  helper or `textContent`, so log entries, quests, and items cannot break
  out of the page — including via a hand-tampered `localStorage` payload;
  `localStorage` is the only storage used, and the storage key named in
  `SECURITY.md` matches the code exactly.
- **Coding Team review — ✅ PASS WITH FIXES, all fixes applied and
  re-verified.** Verified: the XP curve matches the reference formula
  exactly (independently recomputed at levels 2, 10, 50, 92, and 99);
  clean vanilla JS with all user strings escaped; zero external requests;
  `CONFIG` genuinely drives the operator name, title, goal, and currency;
  responsive behavior passes at phone, tablet, and desktop widths. Three
  findings were fixed before publishing: (1) a task-log validation-order
  bug that could persist a phantom entry from a whitespace-only task —
  input is now trimmed and validated before anything is stored, covered by
  an automated smoke test; (2) red accent contrast below WCAG AA — the
  active tab now uses a deep red background (8.23:1) and red text uses a
  brighter token (6.40:1 / 5.97:1 on the panel backgrounds); (3) the
  level-99 skill tile now reads "Max level" instead of a percentage.
  A final automated pass re-checked JS syntax, XP values, the palette,
  storage-key consistency across `index.html`, `README.md`, and
  `SECURITY.md`, and a 21-assertion functional smoke test — all passing.

## Contributing

Forks and pull requests are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).
Please keep the project's core constraint: **vanilla HTML/CSS/JS, one file,
no build step, no external requests.**

## License

MIT — see [LICENSE](LICENSE) for the full text.
