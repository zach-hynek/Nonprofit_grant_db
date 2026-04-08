# Contributing

## Working Style

This project is intended to be open-source, reproducible, and provenance-first. Contributions should improve trust in the data pipeline before they optimize polish.

## Early Contribution Priorities

- ingestion and parser reliability
- schema clarity and documentation
- provenance preservation
- test fixtures for supported filing shapes
- safe monthly re-run behavior

## Pull Request Expectations

- explain what source or schema behavior changed
- include tests when parser or normalization behavior changes
- document new data sources in [DATA_SOURCES.md](DATA_SOURCES.md)
- keep changes narrow enough that provenance and data impact can be reviewed

## Data Handling Rules

- do not commit proprietary third-party data dumps
- do not remove provenance fields for convenience
- prefer conservative parsing when a filing field is ambiguous
- preserve raw text when normalization confidence is low

## Documentation Rules

- update the README when the project shape changes materially
- update the roadmap when a phase exit criteria changes
- document new extractor behavior and supported forms clearly

## Near-Term Setup Expectations

Until the new ingestion stack is scaffolded, contributors should treat the current `crm_app/` directory as legacy code that is not representative of the target architecture.
