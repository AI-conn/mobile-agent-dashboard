# Audit — Mobile Agent Dashboard v1.1

**Date:** 2026-10-09 · **Scope:** `index.html`, `README.md`, `CHANGELOG.md`,
`SECURITY.md` · **Method:** full code read, claim-by-claim README-vs-code
comparison, fresh-user walkthrough, and an automated jsdom smoke suite
(48 assertions, all passing — including two defects this audit caught and
fixed before release, F-08 and F-09).

Severity scale: **High** = breaks the core promise for a typical user ·
**Medium** = real harm or confusion in common use · **Low** = polish,
edge cases, honesty nits.

---

## Findings

### F-01 — HIGH · Progression trapped in one browser — FIXED
All progress lived only in one browser's `localStorage`: clear the cache
and it was gone; nothing moved between a user's phone and PC; there was
no backup story at all. For a template whose promise is "your agent's
progress," that was the biggest blind spot.
**Fix:** Export / Import of the full state as one JSON file (Profile →
At a Glance), with import validated by `sanitizeState` and refused —
no state change — when the shape is wrong.

### F-02 — HIGH · No way for an agent to write progress — FIXED
The only write path was a human typing into the log form. README said
"works with any agent," but an agent had no mechanism to drive the
dashboard; "plug in your agent" meant editing two config lists.
**Fix:** Agent Live Sync, three channels, all built into the code:
(1) in-page bridge API `window.AgentDashboard` (`logWork`,
`setQuestProgress`, `setZoneStatus`, `getState`, `applyFeed`);
(2) cross-tab sync via `BroadcastChannel('mobile-agent-dashboard')`
plus a `storage`-event listener, so every open copy updates instantly;
(3) optional feed polling (`CONFIG.feedUrl`, `CONFIG.feedPollSeconds`)
that merges a JSON feed an external agent publishes. Feed is **off by
default**; with it off the page makes zero network requests, as before.

### F-03 — MEDIUM · Borrowed game identity in the progression framing — FIXED
The level structure presented itself through a well-known game's
identity: a 99 cap as the product's headline ("Level X / 99", "Max
level", max total 2,079) and the genre's famous halfway-milestone
phrasing in the skill detail panel and the README landmarks. The
project rule is that this framing is not ours.
**Fix:** the Core Charge reframe. Skills are cores charging toward
**Full Charge** (13,034,431 XP); tiles and detail panels show charge %
next to the level; the **Half-Charge Mark** (6,517,216 XP) is described
in our own words; max reads "Full Charge ⚡". The exponential curve
math and every v1.0 save are byte-for-byte untouched — presentation
only. README gained "The Core Charge system" section.

### F-04 — MEDIUM · Fresh-user blind spot: demo progress that isn't yours — FIXED
A first-time visitor was greeted with sample log entries, quests, and
items already in place — somebody else's "proof of work" on a dashboard
whose whole law is that progress must be proven. The only escape was
Reset, which restores the demo again.
**Fix:** samples stay (they teach the format and are labeled "Sample"),
and a **Start fresh** button now wipes them to an empty, owned state.

### F-05 — MEDIUM · State validation was shape-only — FIXED
`loadState` checked that arrays existed, nothing more. A hand-edited or
imported state could carry unknown skills, non-standard XP awards,
duplicate inventory slots, or bogus quest statuses and silently corrupt
every total on the page.
**Fix:** `sanitizeState` validates and cleans on **every** entry path
(stored state, import, bridge, feed): known skills only, awards
restricted to 10/25/50/100, unique slots 0–27, known quest statuses,
string length caps, duplicate-id regeneration.

### F-06 — LOW · Zone statuses hardcoded forever — FIXED
Every station zone rendered a hardcoded "Not connected" pill; no input
could ever change it, so the floor could never truthfully light up.
**Fix:** zone reports now live in state (`state.zones`) and are set via
the bridge/feed; the default remains honestly "Not connected."

### F-07 — LOW · README overclaim ("Works with any agent") — FIXED
The claim outran the code (see F-02). The docs now describe the three
real drive paths, include the log entry format and a copy-paste feed
example, and state plainly that feed polling is off by default and what
the one-line `CONFIG` change is. No "automatic" overclaim remains.

### F-08 — HIGH (found by smoke test) · Id-less feed entries double-counted — FIXED
Feed entries without an `id` received a fresh random id on every merge,
so a polled feed re-added them on **every poll** — unbounded phantom XP.
**Fix:** id-less feed entries get a deterministic id hashed from their
content (date + skill + task + xp + note); re-publishing is a no-op.
Documented trade-off in README: two genuinely different tasks with
identical content hash as one — explicit ids are the reliable path.
Bridge calls keep random ids (a live agent logging twice is two tasks).

### F-09 — MEDIUM (found by smoke test) · Zone feed updates never quiesced — FIXED
`applyZoneUpdate` reported a change even when the incoming status was
identical, so any feed containing a zone entry made every poll a
"change" (re-render + storage write forever).
**Fix:** zone updates compare against current state and report no-change.

### F-10 — LOW · Long unbroken strings overflowed rows on narrow phones — FIXED
A task or note with no spaces (a URL, a hash) could push outside its
row. **Fix:** `overflow-wrap:anywhere` on log task/note and detail values.

### F-11 — LOW · Modal never took keyboard focus — FIXED (partial)
Opening a detail panel left focus wherever it was. **Fix:** the close
button is focused on open. A full focus trap is **deferred**: the dialog
is small, closes on Escape and overlay click, and a trap adds real
complexity to a fork-me template — noted here rather than silently skipped.

### F-12 — LOW · SECURITY.md claimed "no network calls" absolutely — FIXED
True for v1.0 and still true by default, but the new opt-in feed made
the absolute wording inaccurate. **Fix:** SECURITY.md now states the
default and the single opt-in exception (only the URL the user sets).

### F-13 — LOW · No screenshots in README — DEFERRED
The README still carries a "screenshots coming soon" TODO. Fabricating
screenshots would violate the project's honesty rules; real ones must
come from a real fork. Left open deliberately.

### F-14 — INFO · Known platform limits, documented not fixed
Feed polling needs an http(s) origin in most browsers (`file://` can
block `fetch`); the bridge and cross-tab sync work regardless — stated
in README. Cross-tab sync was verified by code review and graceful
degradation (no crash where `BroadcastChannel` is unavailable, e.g.
jsdom); a live two-tab check needs a real browser.

---

## Verification (v1.1 release gate)

- JS syntax check on the shipped script: pass.
- XP curve unchanged: L2 = 83, L92 = 6,517,253, L99 = 13,034,431;
  Half-Charge Mark = 6,517,216 — recomputed independently.
- Smoke suite (jsdom, 48 assertions): fresh-user loop via the real form
  (XP accrues, levels and total level move), bridge validation and
  idempotency, feed merge idempotency incl. id-less entries, quest/zone
  updates, export → wipe → import restore, poisoned-import sanitizing,
  stored-XSS escaping, Half-Charge/Full-Charge label rendering at
  6,517,300 XP. **48 passed, 0 failed.**
- Hygiene greps: borrowed-game name 0 hits; the banned halfway phrasing
  0 hits; personal data 0 hits outside the LICENSE copyright line;
  external URLs in `index.html` 0 (the single `fetch` is the opt-in
  feed, guarded by `CONFIG.feedUrl`, empty by default).
- Security posture unchanged: zero external requests by default, all
  user/agent-supplied text rendered through the escaping helper,
  no `eval`/`document.write`, validated imports.
