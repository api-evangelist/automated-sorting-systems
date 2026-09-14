---
name: Publish flight and resource events
description: Write flight lifecycle and resource-allocation events into the Vanderlande OpenAir Datahub flight leg store, and read them back for reconciliation.
api: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
operations: [postFlightScheduledEvent, postFlightTimeEvent, postFlightAircraftAllocationEvent, postStandResourceEvent, postGateResourceEvent, postTerminalResourceEvent, postCheckInDesksResourceEvent, postFlightPaxCountEvent, postFlightCancelledEvent, queryFlightLegs, getFlightLegREST, getFlightLegEvents, queryFlightEvents]
generated: '2026-09-14'
method: generated
source: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
---

# Publish flight and resource events

The flight leg store is the operational spine the baggage stores hang off. Every write is an
append; the flight leg's current state is the fold of its events.

## Steps

1. **Seed the leg.** `postFlightScheduledEvent` — `POST /flightlegs/events/scheduled`. This is
   what makes a flight leg addressable; post it before any resource or time event for that leg.
2. **Keep the times current.** `postFlightTimeEvent` — `POST /flightlegs/events/time`, for
   scheduled/estimated/actual off-block and on-block movement.
3. **Allocate resources as they are assigned.** One operation per resource class, all under
   `/flightlegs/events/`:
   - `postFlightAircraftAllocationEvent` — the aircraft
   - `postStandResourceEvent` — the aircraft parking position
   - `postGateResourceEvent` — the gate
   - `postTerminalResourceEvent` — the terminal
   - `postCheckInDesksResourceEvent` — the check-in desks
   A reallocation is a new event, not an edit of the old one.
4. **Post counts and cancellations.** `postFlightPaxCountEvent` for the passenger count;
   `postFlightCancelledEvent` — `POST /flightlegs/events/cancelled` — when the flight is
   cancelled in the real world.
5. **Reconcile.** `queryFlightLegs` (`GET /flightlegs/`) to find legs by
   `carrier`/`flight-number`/`origin-date`/`origin-iata-code`; `getFlightLegREST`
   (`GET /flightlegs/{id}`) for one leg; `getFlightLegEvents` (`GET /flightlegs/{id}/events`) or
   `queryFlightEvents` (`GET /flightlegs/events`) for the event stream.

## Rules

- **`postFlightCancelledEvent` is a fact, not a rollback.** It says the flight was cancelled. It
  does not undo the resource events you already posted, and there is no operation that does.
  Reversibility for this API is `none` — see
  `conventions/automated-sorting-systems-conventions.yml`.
- **Retries duplicate.** No idempotency key exists. After a timeout, query the leg's events over
  a tight `from`/`to` window and confirm before re-posting.
- **Identify the leg the IATA way.** `carrier` is the 2-letter IATA code of the operating
  carrier, `flight-number` is numeric, and `operational-suffix` is what separates two flights
  sharing a number on the same `origin-date`. Omitting the suffix is the usual cause of an event
  landing on the wrong leg.
- **Do not use `postBagLoadIntoUldEvent`.** It is marked `DEPRECATED!` in the contract with no
  sunset date and no named replacement; use `postBagContainerizationEvent` for load-into-ULD.
