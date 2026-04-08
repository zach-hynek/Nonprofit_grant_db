# Data Sources

## Purpose

This file defines which datasets are part of the project's open-source backbone, which external sources are optional, and which usage patterns are out of bounds.

## Core Required Sources

### 1. IRS EO BMF

Use the Exempt Organizations Business Master File as the canonical organization registry seed.

Expected Phase 1 usage:

- ingest one bounded slice first
- preserve EIN, name, address, subsection, foundation code, and NTEE
- record posting date and record-count metadata when available

### 2. IRS TEOS Form 990 XML Downloads

Use TEOS monthly XML downloads as the main Phase 1 filing source.

Expected Phase 1 usage:

- download one or two monthly ZIPs
- store raw ZIPs and extracted XML immutably
- parse filer identity, tax period, form type, and grant-bearing sections

### 3. IRS TEOS Filing Images / PDFs

These are relevant for future coverage expansion, but they are not a Phase 1 dependency.

Expected future usage:

- OCR and document understanding experiments
- provenance-linked full-text access
- fallback extraction when XML is unavailable

## Optional Sources

Optional sources may be used for convenience, cross-checking, or future enrichments, but they must not become required to rebuild the open dataset.

Examples:

- ProPublica Nonprofit Explorer for spot checks or convenience lookup
- Candid and GrantStation as product and UX references
- additional public nonprofit taxonomies or enrichment layers with compatible terms

## Out Of Bounds

Do not:

- redistribute proprietary third-party datasets as a stand-alone export
- make the project depend on scraped private data
- claim donor-level visibility that is not supported by public disclosures
- hide source ambiguity behind overconfident normalized values

## Provenance Contract

Every derived record exposed through the product should be traceable to:

- dataset name
- source URL or source identifier
- ingestion timestamp
- content hash
- filing period and form type when applicable
- form context or element path when feasible

## Phase 1 Download Set

The initial bounded dataset should be small enough to rebuild locally but rich enough to prove the architecture:

- one EO BMF slice
- one TEOS monthly XML ZIP
- a curated XML fixture set for parser tests

## Review Rule For New Data Sources

Before adding any new source, document:

1. what the source contributes
2. whether its terms allow our intended usage
3. whether the repo can still be rebuilt without it
4. how provenance will be preserved
