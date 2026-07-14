# Architecture (v1)

## Layering

* **UI layer** — screens for obligations, people, types, statistics.
* **Domain layer** — Obligation, Person, and Type models; obligation status and lifecycle rules.
* **Data layer** — local storage, accessed only through a repository interface (see `decisions/002-stable-ids-and-sync-readiness.md`). UI and domain code never call storage APIs directly.

## Why This Layering

This separation is the mechanism behind `decisions/002-stable-ids-and-sync-readiness.md`: v2 (link-based export/import) and v3 (server sync) are added as new implementations of the same repository interface, without changing UI or domain code. It is intentionally the only structural concession v1 makes toward future versions — see the ADR for why more speculative design was avoided.

## Local Storage

The specific storage engine depends on the mobile framework choice, which is not yet made (`decisions/001-mobile-framework.md` is still open). Whatever is chosen, the schema must include the fields defined in `mvp.md` — UUID, status, timestamps — from the start.

## Open Decisions

* `decisions/001-mobile-framework.md` — mobile framework not yet decided.
* Local reminder implementation depends on the notification APIs of the chosen framework/OS.
