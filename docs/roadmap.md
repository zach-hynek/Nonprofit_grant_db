# Roadmap

## North Star

Publish an open-source, reproducible funder database that turns public IRS filings into searchable grant history and recommendation-ready funder intelligence.

## Release Cadence

Use a simple cadence from the start:

- two-week build cycles for implementation work
- one internal milestone at the end of each cycle
- one versioned release whenever a phase exit criteria is met

That keeps the project moving without pretending we can predict every data wrinkle up front.

## Suggested Versioning Shape

- `v0.0.x`: repository foundation and planning docs
- `v0.1.0-alpha`: ingestion backbone works on a bounded dataset
- `v0.2.0-alpha`: grant extraction works with provenance
- `v0.3.0-beta`: search and recommendation API are usable end to end
- `v0.4.0-beta`: hardening, reproducibility, and contributor readiness
- `v1.0.0`: broader coverage, stable interfaces, and operational confidence

## Phase 0: Repository Foundation

Target window: week 1

Deliverables:

- GitHub-facing project docs
- scoped Phase 1 definition
- data-source and terms guardrails
- contributor conventions for provenance, tests, and source handling

Exit criteria:

- a new contributor can understand the problem, scope, and first milestone without reading the full research report
- the repo clearly distinguishes the target architecture from legacy prototype code

Release:

- `v0.0.1`

## Phase 1: Data Backbone Alpha

Target window: weeks 2-3

Deliverables:

- local Postgres + pgvector development setup
- raw source storage layout
- `ingestion_ledger`, `ingest_metadata`, `org_registry`, and `filings` tables
- EO BMF importer for one bounded slice
- TEOS XML downloader and raw ZIP/XML storage

Exit criteria:

- fresh clone can ingest one EO BMF slice and one TEOS monthly ZIP
- ingestion is idempotent enough to rerun without duplicating records
- every stored source object has a hash and source reference

Release:

- `v0.1.0-alpha`

## Phase 2: Grant Extraction Alpha

Target window: weeks 4-5

Deliverables:

- XML parsing for filer identity and tax period
- grant extraction from 990 Schedule I
- grant extraction from 990-PF Part XIV
- `grants` table with provenance fields
- parsing tests on a curated sample set
- ingestion data-quality checks

Exit criteria:

- supported filings produce normalized grant rows
- amounts parse correctly and duplicate grant detection runs
- each grant row links back to filing and form context

Release:

- `v0.2.0-alpha`

## Phase 3: Search And Recommendation Beta

Target window: weeks 6-7

Deliverables:

- `GET /orgs`
- `GET /funders`
- `GET /funders/{ein}`
- `GET /funders/{ein}/grants`
- `POST /recommend/funders`
- embeddings for grant-purpose text using pgvector
- recommendation output with match evidence and citations

Exit criteria:

- users can search organizations and inspect grant history
- recommendation queries return plausible funders with transparent reasons
- the full path from source file to API response is inspectable

Release:

- `v0.3.0-beta`

## Phase 4: Reproducibility And Contributor Beta

Target window: weeks 8-10

Deliverables:

- one-command bootstrap for local setup
- improved fixtures and parser tests
- documented sample ingestion flow
- `DocumentExtractor` interface with `PdfOcrExtractor` stub
- clearer contributor guidance for adding new form mappings

Exit criteria:

- a contributor can clone, ingest the sample slice, run tests, and start the API
- the repo is ready for outside feedback without hidden setup knowledge

Release:

- `v0.4.0-beta`

## Phase 5: Coverage Expansion Toward 1.0

Target window: after beta feedback

Deliverables:

- broader monthly backfill coverage
- stronger entity resolution
- additional form support
- optional OpenSearch integration
- OCR pilot for image-only filings
- operational monitoring for monthly updates

Exit criteria:

- the platform is stable under recurring updates
- the public data model is trustworthy enough for wider usage
- interface changes are uncommon and documented

Release:

- `v1.0.0`

## Recommended Immediate Sprint Order

1. Finish Phase 0 by merging the docs set and agreeing on naming plus license choice.
2. Build Phase 1 around EO BMF and one TEOS monthly XML batch.
3. Add Phase 2 grant extraction before touching any serious frontend work.
4. Ship Phase 3 as the first end-to-end demo worth showing externally.
