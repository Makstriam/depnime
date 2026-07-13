# ADR 002: Stable Identifiers and Sync-Readiness in v1

## Status

Accepted

## Context

The project is deliberately split into three versions (see `roadmap.md`): v1 is fully local, v2 introduces serverless link-based exchange, v3 introduces server-based synchronization. A known risk in staged projects is that early architecture, built with no thought for later stages, has to be substantially rewritten once those stages arrive. The opposite failure mode — fully designing v2/v3 during v1 — is also rejected here; see Rationale.

## Decision

v1 includes exactly these forward-looking elements, and no more:

1. Every obligation is assigned a UUID at creation time.
2. Every obligation has an explicit, structured status (`Active` / `Completed` / `Closed`) and timestamp fields, rather than deriving state from UI or ad hoc fields.
3. Data access goes through a repository abstraction rather than direct storage calls from UI code.

No sharing, linking, account, or sync logic is implemented in v1.

## Rationale

These three elements are cheap to add now and expensive to retrofit later: a UUID cannot be added cleanly after real user data already exists without a migration step, and a repository abstraction is far easier to introduce before UI code is coupled directly to storage calls. Beyond these three points, further speculative design for v2/v3 was deliberately avoided — the actual shape of v2/v3 requirements will only be known after v1 is built and used. Designing for imagined future requirements now (rather than these specific, low-cost hooks) risks building the wrong abstractions, which are typically more expensive to undo than not having them at all.

## Consequences

* v1 has slightly more structure than a minimal throwaway prototype would.
* v2 and v3 are expected to add new modules behind the existing repository interface, not rewrite the data model.
* Some rework between versions is still expected and accepted; this decision reduces but does not eliminate it.
