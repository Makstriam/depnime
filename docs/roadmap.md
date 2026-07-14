# Roadmap

Version 1 (see `mvp.md`) is fully local. This document tracks planned functionality for later versions, grouped by version, so ideas aren't lost but also don't dilute v1 scope.

## Version 2 — Serverless Obligation Exchange

Goal: preserve the shared, two-sided character of an obligation without building a backend.

* Obligations can be shared as an encoded link (containing the obligation's data and its UUID) sent through any third-party messenger.
* Opening the link on the recipient's install creates a matching local obligation, with direction inverted.
* Status changes (e.g. marking an obligation complete) are propagated by generating and re-sharing an updated link. This is a manual re-exchange, not automatic sync — the UI must make this explicit rather than implying real-time synchronization.
* Conflict handling: if both sides changed the obligation independently before exchanging an update, the receiving side should surface the conflict (e.g. "newer wins," shown explicitly) rather than silently overwrite the local copy.
* Deferred deep linking: if the recipient doesn't have the app installed, the link should route to the Play Store first, then open the relevant screen after install. This requires a small static landing page (e.g. GitHub Pages) — not a backend, but not literally zero infrastructure either.

## Version 3 — Accounts and Real Synchronization

Goal: replace manual link re-exchange with a proper backend.

* Real user accounts — created anonymously per-install by default, optionally linkable to an email for access from other devices.
* Server-based synchronization between linked accounts.
* Obligations shared as requests between real accounts: the recipient can confirm, reject, or propose changes (a counteroffer).
* Push notifications: reminders sent to another real user (optionally with a message), and notifications when one party requests to close an obligation.
* Counter Mode increments (see `mvp.md`) push a live notification to the other linked party — e.g. "You now owe 3! Now 4! Now 5!" — this is the core of the app's playful, informal character, not just a functional reminder.
* A request to close an obligation must be confirmed or rejected by the other party. The other party can also permanently hide/dismiss the obligation on their own side if they disagree.
* Either party can propose edits to an obligation at any time.
* Once real personal data (obligations between named real users) is stored on a server, data-protection responsibilities apply (secure storage, breach handling, and GDPR if EU users are involved). This needs its own decision record once v3 is scoped in detail — it is a real operational and legal commitment, not a formality.

## Later / Unscheduled

* Web version — mobile is the priority; web is a valued but non-urgent addition.
* A second visual style/theme (more playful and expressive) alongside the clean/professional default, sharing the same underlying data and functionality.
* iOS release, pending validation of v1/v2 on Android first.
