# Depnime — Project Idea

## Problem

People informally track debts and small obligations between friends, roommates, couples, and family — money, borrowed items, promises — using notes apps, chat history, or memory. These methods are error-prone, don't survive over time, and don't handle non-monetary obligations (returning a book, helping someone move) at all. Existing debt-tracking apps focus narrowly on money and bill-splitting; there is no lightweight tool for casual, informal, non-monetary commitments.

## Core Concept

Depnime tracks **obligations** — a single entity that generalizes "debt" beyond money to include physical items and actions. An obligation is a commitment between the user and another person (in the first version, a locally created Person record, not necessarily a linked real account).

Examples:

* Alex owes me `20 EUR`.
* I owe Maria `3 bottles of cola`.
* Daniel owes me the action `return my book`.

## Differentiation

* Not limited to money: money, items, and actions are all just differently-labeled instances of one underlying Type — not hardcoded, separate categories (see `mvp.md`).
* Custom, reusable types (icon + color + name), so a recurring obligation kind doesn't need to be redefined every time.
* Designed offline-first: fully usable without an account or network connection.
* Long-term direction toward genuine two-sided exchange between real users (see `roadmap.md`), introduced deliberately in stages rather than required from day one.

## Target Users

Casual, informal contexts: roommates, friends, couples, families, and small groups who lend or borrow money and items, or make small promises to each other, and currently rely on memory, notes apps, or chat history to keep track.

## Monetization

Free, supported by ads, with a paid subscription to remove them. Exact mechanics (ad frequency, caps) are an implementation detail, not part of the product vision.

## Versioning Strategy

The project is deliberately split into three versions to manage engineering risk (full detail in `roadmap.md`):

1. **v1 (this MVP)** — fully local, offline-first. No account, no network dependency, no exchange between installs. See `mvp.md`.
2. **v2** — serverless obligation exchange between two installs, via a shareable encoded link sent through any third-party messenger.
3. **v3** — real user accounts, server-based synchronization, and push notifications between users.

A small number of foundational data-model decisions (stable per-obligation identifiers, explicit status values, timestamps, and a storage layer separated from the UI — see `decisions/002-stable-ids-and-sync-readiness.md`) are made starting in v1 specifically so v2 and v3 can be added later without rewriting the core. Beyond these specific decisions, v1 does not attempt to anticipate the rest of v2/v3's design.
