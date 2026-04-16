# TickerForge Repository Guide

## Scope

The top-level `tickerforge` repository is the ecosystem entrypoint. It stays language-agnostic and documents how the repositories fit together.

Implementation-specific design notes should live in the consumer repositories, not here.

## Repository Roles

| Repository | Purpose | Publishes to |
|------------|---------|--------------|
| `tickerforge` | Ecosystem overview, shared principles, and navigation | -- |
| `tickerforge-spec` | Canonical YAML and CSV data, schemas, and language-neutral format documentation | `tickerforge-spec-data` on **PyPI** and **crates.io** |
| `tickerforge-py` | Python implementation and Python-specific integration notes | `tickerforge` on **PyPI** |
| `tickerforge-rs` | Rust implementation and Rust-specific integration notes | `tickerforge` on **crates.io** |

## Published Packages

| Package | Registry | URL |
|---------|----------|-----|
| `tickerforge` | PyPI | https://pypi.org/project/tickerforge/ |
| `tickerforge` | crates.io | https://crates.io/crates/tickerforge |
| `tickerforge-spec-data` | PyPI | https://pypi.org/project/tickerforge-spec-data/ |
| `tickerforge-spec-data` | crates.io | https://crates.io/crates/tickerforge-spec-data |

## Versioning

Each repository versions independently:

- **tickerforge-spec** uses a `VERSION` file at the repo root. The same value drives both the PyPI wheel (`pyproject.toml` via Hatch dynamic version) and the crates.io crate (`Cargo.toml`, kept in sync by `scripts/sync_cargo_version.py`). A `scripts/check_versions.py` CI step verifies all manifests agree.
- **tickerforge-py** uses a `VERSION` file at the repo root, read by Hatch at build time. The `scripts/check_versions.py` CI step validates consistency.
- **tickerforge-rs** uses the `version` field in `Cargo.toml` directly.

Releases are triggered by pushing a `v*` git tag (e.g. `v0.1.3`). The `release.yml` workflow in each repo validates that the tag matches the on-disk version before publishing.

## CI/CD

All three source repositories (`tickerforge-spec`, `tickerforge-py`, `tickerforge-rs`) share the same CI/release pattern:

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `ci.yml` | Push to `main`, pull requests | Lint (fmt, clippy / black, isort, ruff, mypy), test (multi-version matrix), and upload coverage |
| `release.yml` | `v*` tag push | Validate tag-version match, publish to PyPI / crates.io, create GitHub Release |

**Coverage** is collected and uploaded to [Codecov](https://codecov.io) on `tickerforge-py` (pytest-cov) and `tickerforge-rs` (cargo-llvm-cov). The spec repo runs YAML lint and build checks but does not collect coverage.

**Test matrix:**
- Python: 3.10, 3.12, 3.14
- Rust: stable toolchain, MSRV 1.74

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
