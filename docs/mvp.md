# MVP — Version 1 (Local, Offline-First)

## Scope Boundary

Version 1 is fully local. It has no user accounts, no network dependency, and no exchange of obligations between different installs. Reminders are local-only (notifications to yourself), never sent to another user. Sharing obligations between installs is deferred to v2; real accounts and server synchronization are deferred to v3. See `roadmap.md`.

## Core Entity: Obligation

An obligation represents one specific commitment between the user and a Person. One obligation has exactly one type — Money, Item, or Action. If a person owes both money and an item, these are stored as two separate obligations. This keeps individual records simple and makes filtering, statistics, and status tracking predictable.

### Obligation Types

**Money**

* decimal amount
* selected currency — currencies are tracked separately; no automatic conversion
* direction: `I owe` / `Owed to me`
* optional comment

**Item**

* whole-number quantity
* user-defined item (name, icon, color — reusable; see Custom Items below)
* direction: `I owe` / `Owed to me`
* optional comment

**Action**

* title or short description
* direction: `I owe` / `Owed to me`
* optional comment (no numeric quantity)

### Shared Fields (all obligation types)

* stable unique identifier (UUID), assigned at creation — required so v2/v3 can later reference the same obligation without redesigning the data model (see `decisions/002-stable-ids-and-sync-readiness.md`)
* status: `Active` / `Completed` / `Closed`
* creation timestamp
* optional user-defined tag, for personal filtering
* optional comment — informal context (why it was created, what was agreed), not a legal record
* optional one-time reminder (date + time). Recurring reminder rules (weekly repetition, selected weekdays, repeat counts, end dates) may be added within v1 if time allows, otherwise deferred to a later pass

## People

Obligations are linked to locally created Person records. A Person has a name, an optional description, an optional visual identifier, and shows every associated obligation (active, completed, closed) on their page. In v1, a Person is a local record only — not connected to any real Depnime account.

A user may also record an obligation between two people other than themselves (e.g. "X owes Y"), purely as a personal, local note about an arrangement they're aware of.

## Custom Items and Categories

Users can define reusable item types while creating an obligation: a name, an icon selected from a built-in catalogue, and an icon color. Once used, the item is saved to the user's item list and can be reused without re-entering its name, icon, or color. The built-in icon catalogue is grouped into categories such as currencies, food and drinks, household items, books and documents, tools, and general objects.

## Obligation Status

* **Active** — still exists, not resolved.
* **Completed** — fulfilled.
* **Closed** — intentionally abandoned or dismissed without completion.

## Statistics

Displayed per saved item or currency — never combined across incompatible units (money in different currencies is not added together; actions are not included in numeric totals):

* total owed to the user / owed by the user, per currency
* total quantity owed, per item
* active / completed / closed counts, overall and filtered by person, tag, type, or item
* average time to close an obligation

## Offline Requirement

All core functionality must work fully without an account or internet connection: creating and editing obligations, completing or closing them, managing people, managing custom items, viewing history and statistics, and receiving local reminders.

## Foundational Architecture Requirements

These are the minimum forward-looking decisions made in v1 to avoid a rewrite when v2/v3 are built (full rationale in `decisions/002-stable-ids-and-sync-readiness.md`):

* Every obligation is assigned a UUID at creation.
* Status and timestamps are explicit, structured fields — not inferred from UI state.
* Data storage is accessed through a repository abstraction, not called directly from UI code, so v2 (link-based export/import) and v3 (server sync) can be added as new implementations behind the same interface.

No further v2/v3 design is anticipated beyond these three points — see the ADR for why.

## Explicitly Out of Scope for v1

* Any exchange of obligations between different app installs
* User accounts of any kind
* Push notifications to other users
* Currency conversion
* Web version
