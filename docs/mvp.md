# MVP — Version 1 (Local, Offline-First)

## Scope Boundary

Version 1 is fully local. It has no user accounts, no network dependency, and no exchange of obligations between different installs. Reminders are local-only (notifications to yourself), never sent to another user. Sharing obligations between installs is deferred to v2; real accounts and server synchronization are deferred to v3. See `roadmap.md`.

## Type

A Type is a reusable definition, not a fixed built-in category. It consists of:

* **numeric kind** — one of:
  * **Integer** — whole-number quantity (e.g. "3 bottles of cola," "15 flicks on the forehead")
  * **Decimal** — quantity with a fractional part (e.g. money amounts)
  * **Single** — no quantity; the obligation either exists or doesn't (e.g. "return my book")
* **icon** — selected from a built-in catalogue grouped into categories (currencies, food and drinks, household items, books and documents, tools, general objects) for convenience while browsing; the grouping is not a structural constraint
* **icon color**
* **name** — freely chosen by the user

There is no structural distinction between "money," "items," and "actions" — a Euro, a bottle of cola, and a returned favor are all just Types that differ only in numeric kind, icon, color, and name. The icon catalogue may suggest common currencies and items, but a user is free to repurpose any icon under any name.

Once a Type has been used in an obligation, it is saved to the user's Type list and can be reused without re-entering its icon, color, or name.

Quantities are only ever combined within the same Type — two Decimal Types both representing "money" (e.g. EUR and USD) are never auto-converted or added together, same as two different Integer Types are never combined.

## Core Entity: Obligation

An obligation represents one specific commitment between the user and a Person, using exactly one Type. If a person owes both, say, money and an item, these are stored as two separate obligations — this keeps individual records simple and makes filtering, statistics, and status tracking predictable.

### Fields

* Type (see above)
* quantity, matching the Type's numeric kind (omitted for Single)
* direction: `I owe` / `Owed to me`
* stable unique identifier (UUID), assigned at creation — required so v2/v3 can later reference the same obligation without redesigning the data model (see `decisions/002-stable-ids-and-sync-readiness.md`)
* status: `Active` / `Completed` / `Closed`
* creation timestamp
* optional user-defined tag — the primary way to sub-divide obligations that share a Type (e.g. separating "serious" from "joke" obligations that both use the same Integer Type)
* optional comment — informal context (why it was created, what was agreed), not a legal record
* optional one-time reminder (date + time). Recurring reminder rules (weekly repetition, selected weekdays, repeat counts, end dates) may be added within v1 if time allows, otherwise deferred to a later pass

## Counter Mode

For obligations using an Integer Type, the quantity can be adjusted live with a single tap (+1 / -1), instead of being fixed once at creation. This supports situations where the final count isn't known in advance — for example, creating an obligation for "0 Snickers bars" before a game and tapping +1 each time a point is won, so the total is tracked live without being held in memory or calculated afterward.

In v1, Counter Mode is purely local — adjusting the count doesn't notify anyone. See `roadmap.md` (v3) for the networked version, where increments push a live notification to the other party.

## People

Obligations are linked to locally created Person records. A Person has a name, an optional description, an optional visual identifier, and shows every associated obligation (active, completed, closed) on their page. In v1, a Person is a local record only — not connected to any real Depnime account.

A user may also record an obligation between two people other than themselves (e.g. "X owes Y"), purely as a personal, local note about an arrangement they're aware of.

## Obligation Status

* **Active** — still exists, not resolved.
* **Completed** — fulfilled.
* **Closed** — intentionally abandoned or dismissed without completion.

## Statistics

Displayed per Type — never combined across different Types, even if both happen to represent money:

* total owed to the user / owed by the user, per Type
* active / completed / closed counts, overall and filtered by person, tag, or Type
* average time to close an obligation

## Offline Requirement

All core functionality must work fully without an account or internet connection: creating and editing obligations, completing or closing them, managing people, managing Types, viewing history and statistics, receiving local reminders, and using Counter Mode.

## Foundational Architecture Requirements

These are the minimum forward-looking decisions made in v1 to avoid a rewrite when v2/v3 are built (full rationale in `decisions/002-stable-ids-and-sync-readiness.md`):

* Every obligation is assigned a UUID at creation.
* Status and timestamps are explicit, structured fields — not inferred from UI state.
* Data storage is accessed through a repository abstraction, not called directly from UI code, so v2 (link-based export/import) and v3 (server sync) can be added as new implementations behind the same interface.

No further v2/v3 design is anticipated beyond these three points — see the ADR for why.

## Explicitly Out of Scope for v1

* Any exchange of obligations between different app installs
* User accounts of any kind
* Push notifications to other users (including Counter Mode updates)
* Currency conversion
* Web version
