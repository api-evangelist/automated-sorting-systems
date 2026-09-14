---
name: Track ULDs and containers
description: Register, move, store and secure unit load devices in the Vanderlande OpenAir Datahub container tag store, and read their event history.
api: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
operations: [postContainerRegistrationEvent, postContainerMovementEvent, postContainerStorageEvent, postContainerSecurityEvent, postContainerContainerizationEvent, queryContainertags, getContainerTagREST, queryContainerEventsForContainerTag, queryContainerEvents]
generated: '2026-09-14'
method: generated
source: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
---

# Track ULDs and containers

The container tag store mirrors the bag tag store one level up: a ULD is registered once, then
accumulates events. `container-tag` is the query key.

## Steps

1. **Register the ULD.** `postContainerRegistrationEvent` — `POST /containertags/events/registration`.
2. **Record what happens to it.** All under `/containertags/events/`:
   - `postContainerMovementEvent` — it moved
   - `postContainerStorageEvent` — it went into or out of store
   - `postContainerSecurityEvent` — a security action was taken on it
   - `postContainerContainerizationEvent` — bags were built into it
3. **Find it.** `queryContainertags` — `GET /containertags/` — filter on `container-tag`,
   `carrier`, `flight-number`, `origin-date`, `origin-iata-code`, `destination-iata-code`, and
   bound with `created-from`/`created-to`.
4. **Read the history.** `getContainerTagREST` (`GET /containertags/{id}`) for the record,
   `queryContainerEventsForContainerTag` (`GET /containertags/{id}/events`) for its events, or
   `queryContainerEvents` (`GET /containertags/events`) across containers.

## Rules

- **Cross-check against the bag side.** A containerization event exists on both stores
  (`postBagContainerizationEvent` and `postContainerContainerizationEvent`). Post the one that
  matches the subject of the fact you are recording; posting both for one physical action
  double-counts, and nothing in the API deduplicates.
- **CPM messages write here too.** `parserCpm` (`POST /parser/cpm`) parses container/pallet
  distribution messages into this store. If an upstream system already feeds CPMs, posting the
  same movements again through `/containertags/events/` duplicates them.
- **Append-only, no undo, no idempotency key.** Same as every other write surface in this
  contract.
- **Default 24-hour window and a 1000-record cap** apply to every query here. Set
  `created-from`/`created-to` explicitly.
