# Product Brief

## Product Vision

Build an open-source funder intelligence layer that helps nonprofits, researchers, and journalists search philanthropic funders and discover likely matches using public IRS filings.

The system should feel trustworthy before it feels magical. Search and recommendations must be grounded in observable grant history, not opaque scoring.

## Core Problem

Public nonprofit filings already contain valuable funding relationships, but they are difficult to use directly because:

- filing availability is delayed and uneven
- grant data lives across different forms and sections
- recipient identity is sometimes ambiguous
- purpose text is messy and inconsistent
- most public tools hide provenance behind polished profiles

## Primary Users

- nonprofit teams researching likely funders
- grant writers and development staff looking for comparable grant patterns
- journalists and researchers analyzing funding networks
- maintainers and contributors building an open-source public-data stack

## Phase 1 Outcome

From a fresh clone, a contributor should be able to:

1. download a bounded set of IRS source files
2. build a local database from those files
3. query organizations and grants
4. submit a short project description and receive funder recommendations backed by citations

## Phase 1 Success Criteria

- EO BMF ingestion creates a stable canonical organization registry
- one TEOS monthly XML batch is stored raw, parsed, and linked to filings
- grant rows are extracted from 990-PF Part XIV and 990 Schedule I
- organization search and grant-purpose search both work
- semantic retrieval over grant-purpose text returns at least one useful recommendation result
- every returned record includes source and filing provenance
- ingestion can be rerun safely without duplicating records

## Core Objects

### Organization Registry

Canonical organization record keyed by EIN and sourced from EO BMF.

### Filing

A single IRS return with tax period, form type, raw object reference, and ingestion provenance.

### Grant

A normalized funder-to-recipient transaction with amount, purpose text, form context, and traceable source links.

### Text / Embedding Record

The text used for retrieval, plus the vector needed for semantic matching and recommendation.

## Core Workflows

### 1. Ingest Authoritative Public Data

- download EO BMF and TEOS XML sources
- hash and store raw source artifacts
- record ingestion status and metadata

### 2. Normalize Grant Relationships

- map filings to canonical filers
- extract grant rows from supported forms
- preserve raw recipient fields when joins are incomplete
- attach provenance to every derived row

### 3. Search And Recommend

- search organizations by name and filters
- search grants by purpose text and filing metadata
- recommend funders based on semantic similarity to a project description

## Product Principles

- provenance first
- reproducible rebuilds
- monthly update friendliness
- open-source friendly data posture
- XML-first before OCR-heavy expansion

## Non-Goals For Phase 1

- donor-level traceability through confidential schedules
- full historical backfill
- comprehensive OCR over image-only PDFs
- aggressive entity resolution beyond EIN joins and conservative matching
- polished end-user workflow software beyond the minimal API and validation tooling

## Direction After Phase 1

- broader TEOS backfill
- additional grant-bearing form support
- OCR lane for image-only filings
- stronger recipient resolution and deduplication
- dedicated search infrastructure for large-scale faceting
- richer user-facing application layers on top of the data platform
