---
name: Ingest IATA Type B baggage messages
description: Post BSM, BPM, BUM and CPM messages into the Vanderlande OpenAir Datahub parser stores and read back the parsed results, with the retry and replay rules the contract does not give you.
api: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
operations: [parserBsm, parserBpm, parserBum, parserCpm, getBsms, getBpms, getBums, getCpms, getBsmById, getBpmById, getBumById, getCpmById]
generated: '2026-09-14'
method: generated
source: openapi/automated-sorting-systems-vanderlande-datahub-openapi.yml
---

# Ingest IATA Type B baggage messages

The Datahub exposes the four IATA Type B baggage message families as first-class endpoints. Each
family has a POST that parses and stores, a GET that queries, and a GET by id.

| Message | Post | Query | By id |
|---|---|---|---|
| BSM — Baggage Source Message | `parserBsm` `POST /parser/bsm` | `getBsms` `GET /parser/bsm` | `getBsmById` |
| BPM — Baggage Processed Message | `parserBpm` `POST /parser/bpm` | `getBpms` `GET /parser/bpm` | `getBpmById` |
| BUM — Baggage Unload Message | `parserBum` `POST /parser/bum` | `getBums` `GET /parser/bum` | `getBumById` |
| CPM — Container/Pallet Distribution Message | `parserCpm` `POST /parser/cpm` | `getCpms` `GET /parser/cpm` | `getCpmById` |

## Steps

1. **Check the connection first.** `doPing` — `GET /reportflight/ping`. This is the only test
   operation in the contract; there is no sandbox, no test mode and no magic test identifiers.
   Whatever host you are pointed at is a real store.
2. **Post the raw message.** Call the matching `parser*` operation with the Type B message body.
   The parser writes the structured result into the bag tag / container tag stores, so a
   successful POST changes what the query skills will see.
3. **Read back what was parsed.** Call the matching `get*` operation, or `get*ById` with the id
   the POST returned, and confirm the parse produced the fields you expected before you treat
   the ingest as done. Nothing else confirms it.

## Rules

- **There is no replay protection. Verify before you retry.** No `Idempotency-Key` header or
  parameter exists on any of the 25 mutating operations in this contract. A retried POST after a
  timeout is indistinguishable, to the Datahub, from a second real message. On an ambiguous
  failure, call the matching `get*` query over a tight `from`/`to` window and check whether your
  message already landed — do not blind-retry.
- **There is no way to take it back.** The contract has no delete, cancel, void or reverse
  operation for a parsed message, and no `DELETE` method at all. A bad ingest is corrected by
  the operator, out of band.
- **Error handling.** `400 Bad Request` is declared on every parser operation and carries a
  Spring `ModelAndView` body under `*/*`. A malformed Type B message and a malformed HTTP
  request are not distinguishable from the status code alone; read the body.
- **Standards context.** The Datahub's schemas follow IATA AIDM and ACI ACRIS — see
  `conformance/automated-sorting-systems-conformance.yml`. Field names and identifier formats
  (carrier codes, flight numbers, operational suffix, origin date) follow IATA conventions, so
  reuse your existing IATA validation rather than inventing new ones.
