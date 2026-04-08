# Architecture Notes

## Architectural Thesis

This project should be built as a pipeline-first data platform, not as a UI-first application.

The critical path is:

1. ingest authoritative IRS datasets
2. store raw artifacts immutably
3. normalize filings and grants into a stable relational model
4. expose search and recommendation as views over that model
5. add OCR and richer product layers only after the XML path is solid

## Design Principles

- deterministic rebuilds from upstream public sources
- idempotent monthly updates
- provenance on every derived field that reaches the user
- conservative normalization when source data is ambiguous
- clear interface boundaries so search, embeddings, and extraction layers can evolve independently

## Phase 1 System Shape

### Storage Layers

#### 1. Raw Object Storage

Store downloaded EO BMF files, TEOS ZIPs, and extracted XML documents with hashes and stable local paths.

#### 2. Canonical Relational Store

Use Postgres as the source of truth for normalized records and analytics-friendly joins.

#### 3. Vector Retrieval Layer

Use `pgvector` inside Postgres for the first semantic retrieval workflow so embeddings live next to the canonical data.

## Canonical Tables

### `ingestion_ledger`

Tracks dataset name, source URL, download time, content hash, parse status, and notes about retries or failures.

### `ingest_metadata`

Stores dataset-level snapshots such as IRS posting date, record count, and batch-level run metadata.

### `org_registry`

Canonical organization registry sourced from EO BMF with EIN, name, address, NTEE, subsection, foundation code, and effective metadata where available.

### `filings`

One row per IRS return with filer EIN, tax period, form type, raw object reference, and ingestion provenance.

### `grants`

Normalized grant rows extracted from supported filing sections. At minimum:

- funder EIN
- recipient EIN when present
- raw recipient name and address
- cash and noncash amounts
- purpose text
- form context
- filing reference

### `texts` or `embeddings`

Stores the text used for retrieval and the corresponding embedding vector.

## Phase 1 Extraction Strategy

### Lane A: XML First

This is the real implementation path for Phase 1.

- ingest one or two TEOS monthly XML ZIPs
- parse filer identity and tax period
- extract grants from:
  - Form 990 Schedule I
  - Form 990-PF Part XIV

### Lane B: PDF / OCR Later

This is a planned extension point, not a Phase 1 deliverable.

- keep an extractor interface ready for PDF-based implementations
- document OCRmyPDF, Tesseract, layout detection, and table extraction as future work
- treat OCR outputs as confidence-scored and reviewable

## Service Boundaries

### Source Downloader

Knows where files came from and records hashes plus source metadata.

### Document Extractor

Shared interface for converting a raw filing artifact into structured records.

- `XmlExtractor` is the only required implementation in Phase 1.
- `PdfOcrExtractor` should exist only as a stub or placeholder.

### Normalization Layer

Maps raw filing values into canonical tables while preserving unresolved source text where needed.

### Search Layer

Starts with database-native full-text search and can later swap in OpenSearch behind the same interface.

### Recommendation Layer

Uses similarity against embedded grant-purpose text, then aggregates matching grants back to recommended funders with citations.

## Proposed Phase 1 Request Flow

1. user searches an organization or funder
2. API queries `org_registry` and related `filings` / `grants`
3. user submits a project description
4. service embeds the text
5. vector query finds similar historical grant purposes
6. results are aggregated by funder and returned with provenance

## What We Should Not Build First

- a polished frontend before the data backbone exists
- complex entity resolution before EIN-based joins are stable
- broad OCR coverage before XML extraction is trustworthy
- a search cluster before Postgres full-text and pgvector are proven insufficient

## Honest Repo Note

The current `crm_app/` directory reflects a legacy prototype from an earlier project direction. It should be treated as temporary baggage while we scaffold the ingestion-driven architecture described above.
