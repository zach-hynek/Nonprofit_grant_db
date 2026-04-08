# Open Funder Graph

Open Funder Graph is a provenance-first, open-source data platform for building a searchable database of philanthropic funders and grants from IRS public filings.

The first build target is deliberately narrow: ingest a bounded slice of IRS source data, normalize it into a stable relational schema, and expose search plus a first recommendation workflow over historical grant behavior.

## Why Start Here

The hard part of this project is not collecting forms. It is turning delayed, messy, partially redacted filings into a reliable graph of:

- funders
- recipients
- grant purposes
- amounts
- filing provenance

That means the right first step is a docs-first project contract, followed immediately by a thin vertical slice of ingestion and normalization.

## Current Repository Status

This repository is being repurposed around the funder database project. The existing `crm_app/` code is legacy prototype work from an earlier direction and should not be treated as the target architecture for Open Funder Graph.

The documents in this repo now define the product, source-of-truth boundaries, and release plan for the new system.

## First Implementation Milestone

The first coding milestone is:

1. ingest one EO BMF slice into `org_registry`
2. ingest one TEOS monthly XML ZIP into raw storage plus `filings`
3. extract a small set of grant rows into `grants`
4. prove one search flow and one recommendation flow with provenance

If we can do that reproducibly from a fresh clone, the project has a real foundation.

## Engineering Principles

- Prefer IRS TEOS and EO BMF as the core public data backbone.
- Keep every derived row traceable to a source file, filing period, and form context.
- Design for monthly incremental updates and full rebuilds.
- Use XML-first extraction before investing in OCR.
- Do not redistribute proprietary third-party datasets.

## Project Map

- [Product Brief](docs/product-brief.md)
- [Architecture](docs/architecture.md)
- [Roadmap And Release Cadence](docs/roadmap.md)
- [Data Sources And Terms Guardrails](DATA_SOURCES.md)
- [Contributing](CONTRIBUTING.md)

## Suggested Next Build Step

Start by scaffolding the ingestion backbone, not the final UI:

- Postgres + pgvector dev environment
- raw source storage layout
- ingestion ledger
- `org_registry`, `filings`, and `grants` tables
- one EO BMF importer and one TEOS XML parser

That is the smallest slice that validates the architecture described in the research report.
