# Ecosystem README and Repository Guide Overhaul

## Current State

- `README.md` is 15 lines, says "Publishing soon" to PyPI/crates.io -- both are now live
- `REPOSITORY_GUIDE.md` is 38 lines, describes repo roles but has no mention of publishing, CI, or versioning

## Published Package Facts

| Package | Registry | Version | URL |
|---------|----------|---------|-----|
| `tickerforge` | PyPI | 0.1.3 | https://pypi.org/project/tickerforge/ |
| `tickerforge` | crates.io | 0.1.4 | https://crates.io/crates/tickerforge |
| `tickerforge-spec-data` | PyPI | 0.1.1 | https://pypi.org/project/tickerforge-spec-data/ |
| `tickerforge-spec-data` | crates.io | 0.1.5 | https://crates.io/crates/tickerforge-spec-data |

CI runs on all 3 repos (`tickerforge-spec`, `tickerforge-py`, `tickerforge-rs`) with Codecov coverage on py and rs.

---

## 1. Rewrite `README.md`

Replace the current 15-line stub with a proper hub page. Structure:

- **Header**: project name, one-line description, dynamic badges for both PyPI and crates.io versions (using shields.io `/pypi/v/` and `/crates/v/` endpoints), plus MIT license badge
- **Overview**: 2-3 sentence description of what TickerForge is (derivatives symbol resolution, multi-market, spec-driven)
- **Ecosystem diagram**: a mermaid flowchart showing how `tickerforge-spec` feeds data into `tickerforge-py` and `tickerforge-rs`, and how the spec-data packages bridge them
- **Packages section**: a sub-section per package with:
  - `tickerforge-spec` (data): what it contains, install for Python (`pip install tickerforge-spec-data`) and Rust (`cargo add tickerforge-spec-data`), link to repo
  - `tickerforge-py`: what it does, install (`pip install tickerforge`), Python versions (3.10/3.12/3.14), link to repo, CI/coverage badges
  - `tickerforge-rs`: what it does, install (`cargo add tickerforge`), MSRV (1.74), link to repo, CI/coverage badges
- **Feature summary**: bullet list of capabilities shared across implementations (multi-market futures/options parsing, ticker generation, contract-centric API, exchange calendars, expiration resolution, ambiguity detection)
- **Supported markets**: list B3 and CME with note that adding a market is just a YAML file drop
- **Link to REPOSITORY_GUIDE.md** for repo structure details
- **License**: MIT

Dynamic badge URLs to use:
- `https://img.shields.io/pypi/v/tickerforge` (Python version)
- `https://img.shields.io/crates/v/tickerforge` (Rust version)
- `https://img.shields.io/pypi/v/tickerforge-spec-data` (spec Python)
- `https://img.shields.io/crates/v/tickerforge-spec-data` (spec Rust)
- Codecov badges from each repo's existing badge URLs
- CI badges from each repo's existing GitHub Actions URLs

## 2. Update `REPOSITORY_GUIDE.md`

Keep the existing structure but expand with:

- **New section: "Published Packages"** -- table listing all 4 published artifacts (tickerforge on PyPI, tickerforge on crates.io, tickerforge-spec-data on PyPI, tickerforge-spec-data on crates.io) with registry links
- **New section: "Versioning"** -- explain that `tickerforge-spec` has a `VERSION` file that drives both the PyPI wheel and crates.io crate; implementation repos version independently via `VERSION` (Python) or `Cargo.toml` (Rust); releases are triggered by `v*` git tags
- **New section: "CI/CD"** -- note that all 3 repos have `ci.yml` (lint + test + coverage) and `release.yml` (tag-triggered publish to PyPI / crates.io + GitHub Release); Codecov integration on py and rs
- **Update "Repository Roles"** -- add a brief note about each repo's publishing target (tickerforge-spec publishes spec-data to both PyPI and crates.io; tickerforge-py publishes tickerforge to PyPI; tickerforge-rs publishes tickerforge to crates.io)
