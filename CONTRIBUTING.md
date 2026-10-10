# Contributing to Mobile Agent Dashboard

Thanks for your interest. This project is built to be forked — the best
contribution is often your own themed version. Improvements to the template
itself are welcome too.

## Fork first

1. Fork the repo on GitHub.
2. Clone your fork:

   ```bash
   git clone https://github.com/<your-username>/mobile-agent-dashboard.git
   ```

3. Make your changes, test them by opening `index.html` in a browser (or
   serving the folder statically), and push to your fork.
4. If you'd like the change in the template itself, open a pull request
   against the original repo's default branch.

## Ground rules

- **One file, vanilla, no build step.** The app is a single `index.html`
  with inline CSS and JS. No frameworks, no bundlers, no npm dependencies,
  no external CDN links, no web fonts. If it needs a network request to
  run, it doesn't belong here.
- **No personal data.** Never commit real names, addresses, emails,
  account details, or real earnings. Demo data only, clearly demo.
- **Truthful telemetry.** Features must not simulate work, activity, or
  numbers. If the app shows it, it must be derived from the user's own
  logged data.
- **Palette discipline.** Green, red, and white on dark neutrals, via the
  CSS variables in `:root`. No new accent colors without discussion in
  an issue first.

## Code style

- Plain, readable vanilla JS (ES2020 is fine). Small functions, obvious names.
- All user-facing text inserted into the DOM must go through the `esc()`
  helper (or `textContent`) — no raw HTML injection of stored data.
- State shape changes need a note in the PR: the app persists to
  `localStorage`, so breaking the schema strands existing users' data.
  Bump the storage key version if the shape must change.
- CSS: use the existing theme tokens; avoid hard-coded colors.
- Keep comments short and useful. Match the surrounding style.

## Proposing changes

- For anything bigger than a fix, open an issue first describing the
  change and why it belongs in a template others will fork.
- One topic per pull request. Say what you changed, how you tested it
  (browser + what you clicked through), and whether stored data is affected.
- Screenshots help a lot for UI changes.

## Reporting bugs

Open an issue with: what you did, what you expected, what happened, and
your browser. If the bug involves stored data, export nothing sensitive —
describe the shape of the data instead.
