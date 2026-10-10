# Security Policy

## How this app handles data

Mobile Agent Dashboard (template edition) is a single static HTML file. It has:

- **No backend, no accounts, no analytics, and no network calls by
  default.** The one exception is opt-in: if you set `CONFIG.feedUrl`
  (Agent Live Sync), the page polls that single URL — one you chose and
  control — to merge progress your agent publishes. Nothing else is
  ever fetched, and with `feedUrl` empty (the default) the page makes
  zero network requests.
- **No data collection.** Everything you enter — task log entries, quests,
  inventory items, goal numbers — is stored only in your own browser's
  `localStorage`, on your own machine, under the key
  `mobile-agent-dashboard-template-v1`.
- Nothing is transmitted anywhere. Closing the page sends nothing.

That also means: **your data is only as safe as your browser profile.**
Anyone with access to your unlocked browser can open the file and see
your station, and clearing site data deletes it. Don't store secrets in
it — no passwords, keys, recovery phrases, or personal identifiers. The
station is for operational status, not credentials.

## Reporting a vulnerability

If you find a security issue — for example, a way for stored data to break
out of the page (HTML/JS injection via a log entry, quest, or item), or
anything that makes the app load external resources:

1. Please do **not** post exploit details in a public issue.
2. Use GitHub's **private vulnerability reporting** on this repository
   (Security tab → "Report a vulnerability"), or open a minimal issue
   saying you have a security report to make privately and a maintainer
   will follow up.

Please include: a short description, steps to reproduce with harmless
demo data, and your browser/OS. Reports are acknowledged as soon as
practicable; fixes ship as a new commit to the default branch and a
note in the CHANGELOG.

## Supported versions

Only the latest version on the default branch is supported. Because the
app is a static file, "upgrading" means pulling the newest `index.html`;
your `localStorage` data carries over unless a release note says the
storage schema changed.
