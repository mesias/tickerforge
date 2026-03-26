# TickerForge Repository Guide

## Scope

The top-level `tickerforge` repository is the ecosystem entrypoint. It stays language-agnostic and documents how the repositories fit together.

Implementation-specific design notes should live in the consumer repositories, not here.

## Repository Roles

- `tickerforge`: ecosystem overview, shared principles, and navigation
- `tickerforge-spec`: canonical YAML and CSV data, schemas, and language-neutral format documentation
- `tickerforge-py`: Python implementation and Python-specific integration notes
- `tickerforge-rs`: Rust implementation and Rust-specific integration notes

## Documentation Placement

Use this rule when deciding where a document belongs:

- Put it in `tickerforge-spec` if it defines canonical data shape, validation rules, or consumer-facing semantics.
- Put it in `tickerforge-py` or `tickerforge-rs` if it describes loaders, runtime models, adapters, caches, or API behavior in one language.
- Put it in `tickerforge` only if it remains agnostic about implementation language and primarily explains repository boundaries, shared goals, or ecosystem structure.

## Current Split

Examples of language-neutral spec docs:

- rule-based exchange schedules
- session segment semantics
- how to add a new exchange to the canonical data
- spec release and packaging steps for the spec artifact itself

Examples of implementation-specific docs:

- how Python registers spec schedules into its calendar layer
- how Rust maps YAML session segments into runtime structs
- backend choices, caching, and fallback behavior inside one implementation
