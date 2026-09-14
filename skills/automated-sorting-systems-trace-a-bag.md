---
name: Trace a bag through an airport
description: Follow a single bag from check-in to load, using the Vanderlande OpenAir Datahub bag tag store — resolve the tag, read its flight segments, then walk its event history.
api: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
operations: [queryBagTags, getBagTagREST, queryFlightSegmentBagTagsForBagTag, queryBagEventsForBagTag]
generated: '2026-09-14'
method: generated
source: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
---

# Trace a bag through an airport

The Datahub bag tag store is append-only. You resolve a bag to an internal id, then read the
events that were written against it. Base URL `https://api.datahub.oair.io`.

## Before you start

- **Credentials are not in the contract.** The published OpenAPI declares no `securitySchemes`
  and no `security[]` requirement. Vanderlande issues access through
  <https://www.vanderlande.com/airport-logistics/developerportal-contact/>; ask the operator
  which credential the deployment expects and how to present it. Do not assume anonymous access.
- **Everything is a query, nothing is a page.** There is no cursor and no offset. `max-results`
  caps a response (default 1000 records) and there is no link to the next page. If you hit the
  cap, narrow the time window and re-query — do not assume you saw everything.
- **Default window is 24 hours.** With no query parameter set, the search is reduced to the last
  24 hours. State your window explicitly.

## Steps

1. **Find the bag.** `queryBagTags` — `GET /bagtags/`. Filter on the printed licence plate with
   `tag`, or on the flight with `carrier` (2-letter IATA), `flight-number`,
   `operational-suffix`, `origin-date`, `origin-iata-code`, `destination-iata-code`. Bound it
   with `created-from`/`created-to` or `updated-from`/`updated-to` in ISO-8601 UTC
   (`2020-01-31T12:30:00.000Z`).
2. **Pin the record.** Take the Datahub bag-tag id from step 1 and call `getBagTagREST` —
   `GET /bagtags/{id}`. The id is assigned by the Datahub (ADH), not printed on the tag; a tag
   number can resolve to more than one record across origin dates.
3. **Read its itinerary.** `queryFlightSegmentBagTagsForBagTag` — `GET /bagtags/{id}/segments`
   returns the flight segments the bag is booked on. Use `queryFlightSegmentBagTags`
   (`GET /bagtags/segments`) instead when you want every bag on one segment.
4. **Walk the history.** `queryBagEventsForBagTag` — `GET /bagtags/{id}/events`. Filter with
   `event-type`, `location`, `source` and the `from`/`to` window. The event types you will see
   map to the write operations: check-in, tracking, screening, containerization, change,
   re-flight, re-route and cancel.

## Rules

- **`BagCancelEvent` is not an undo.** It records that the real bag was cancelled. Nothing in
  this API reverses a prior call — there is no `DELETE` method anywhere in the contract and no
  cancel/void/refund operation. Treat every write as permanent.
- **Errors are not problem+json.** A `400` returns a Spring `ModelAndView` body under `*/*`, not
  `application/problem+json`, and no `5xx` is declared on any operation. Parse defensively and
  do not rely on a typed error envelope. See
  `errors/automated-sorting-systems-problem-types.yml`.
- **No idempotency key exists.** If you also write events, see the ingest skill — a retry is a
  second event, not a replay.
