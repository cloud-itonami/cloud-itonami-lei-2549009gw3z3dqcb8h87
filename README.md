# cloud-itonami-lei-2549009gw3z3dqcb8h87

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Expedia Group, Inc..**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Expedia Group, Inc.**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Expedia Group, Inc.
- **LEI (ISO 17442)**: [2549009GW3Z3DQCB8H87](https://search.gleif.org/#/record/2549009GW3Z3DQCB8H87) (GLEIF-verified)
- **Jurisdiction**: US-DE
- **Website**: https://www.expediagroup.com
- **Ticker**: EXPE (NASDAQ)


## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 10 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and ten facts recorded — the LEI record
(legal name as GLEIF spells it, **`Expedia Group, Inc.`**; entity **ACTIVE**,
registration **ISSUED**, `FULLY_CORROBORATED` / `CONFORMING`; entity status and
registration status are different fields and are recorded separately; legal
address `c/o NRAI, 1209 Orange Street, 19801, Wilmington, US-DE` and
headquarters `1111 Expedia Group Way West, Attn: Legal, 98119, Seattle, US-WA`
as GLEIF spells them; entity creation date `2005-04-18`; initial LEI
registration date `2026-05-01`, which is what the cited record says and is
recorded as such — an LEI's registration date is not the company's), its ISIN
mapping (**zero** — a measured zero read from `meta.pagination.total` of the
cited page: GLEIF maps no instrument identifier to this LEI; the EXPE common
stock ISIN `US30212P3029` resolves to no LEI at all in GLEIF's ISIN lookup, and
the two instruments GLEIF does map in this group sit on the child below — neither
observation is in `facts.edn`, because the generator records only what it cites
per entity), its managing LOU and LEI-issuer accreditation (Bloomberg Finance
L.P.), registration authority `RA000602` (the Delaware Division of Corporations,
entry `3956616`, which GLEIF records as OpenCorporates id `us_de/3956616`), ISO 20275
legal form `XTIQ` (Delaware corporation), reporting exceptions at both
consolidation levels (`NATURAL_PERSONS` — GLEIF's code for an entity whose
controlling interest is held by natural persons rather than a consolidating
legal entity, so there is no parent to report), and **one direct child**
recorded as its own entity: Expedia, Inc. (US-WA). Nine of the eleven URLs
answered `200` when the file was written; the `direct-parent` and
`ultimate-parent` endpoints answered `404` because GLEIF publishes the exception
side of that pair for this entity, which the checker treats as a fact rather
than a failure.

The cited LEI record carries no `otherNames`, no `otherAddresses`, no successor
entities and no event groups for this entity, so unlike some siblings in this
family there is nothing the record says that `facts.edn` leaves out.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value drifted
(each difference is named, with the recorded and live values side by side), and
`3` when the check could not be performed at all — `facts.edn` missing or empty,
or GLEIF unreachable at the transport level — because a check that could not run
must not look like a check that ran and found nothing. Before this landed, all
three were shown against the live API: unmodified → `0`; `:company/jurisdiction`
edited from `US-DE` to `US-WA` → `1`, naming `gleif-lei-record
:company/jurisdiction`; the direct-child entity deleted → `1`, naming it as
`ADDED`; `fetch` made to fail with `ENOTFOUND` → `3`; `facts.edn` absent → `3`.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
