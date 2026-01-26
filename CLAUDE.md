# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this codebase.

## Project Overview

DataScript is an immutable in-memory database and Datalog query engine for Clojure and ClojureScript. It's designed to run in the browser as a lightweight, ephemeral data structure with database-like querying capabilities.

## Build System

The project uses both Leiningen (`project.clj`) and Clojure CLI (`deps.edn`):

- **Leiningen**: Primary for ClojureScript builds and releases
- **deps.edn**: Primary for Clojure development and testing

## Common Commands

### Testing

```bash
# Run Clojure tests
./script/test_clj.sh

# Run ClojureScript tests (requires lein and node)
./script/test_cljs.sh

# Run JavaScript API tests
./script/test_js.sh

# Run all tests
./script/test_all.sh
```

### Development REPL

```bash
./script/repl.sh
# Or: clojure -A:dev
```

### Building

```bash
# Build release JS (advanced compilation)
lein with-profile test cljsbuild once release

# Build advanced JS for tests
lein with-profile test cljsbuild once advanced

# Clean build artifacts
./script/clean.sh
```

### Benchmarking

```bash
./script/bench_clj.sh    # Clojure benchmarks
./script/bench_cljs.sh   # ClojureScript benchmarks
./script/bench_all.sh    # All benchmarks
```

## Project Structure

```
src/datascript/
├── core.cljc        # Main public API
├── db.cljc          # Database implementation (largest file)
├── query.cljc       # Datalog query engine
├── parser.cljc      # Query parser
├── pull_api.cljc    # Pull API implementation
├── conn.cljc        # Connection/atom wrapper
├── serialize.cljc   # Serialization support
├── storage.clj      # JVM storage implementation
└── impl/entity.cljc # Entity implementation

test/datascript/test/  # Test files mirror src structure
docs/                  # Additional documentation (queries, tuples, storage)
```

## Key Architectural Notes

- Files use `.cljc` extension for cross-platform Clojure/ClojureScript compatibility
- The database is immutable - transactions create new DB values
- Uses persistent-sorted-set for indexes (EAVT, AEVT, AVET)
- No schema required for attributes unless special behavior is needed
- Reflection warnings are enabled (`*warn-on-reflection*`)

## Dependencies

Core dependencies:
- `persistent-sorted-set`: For sorted indexes
- `io.github.tonsky/extend-clj`: Clojure extension utilities

## Testing Notes

- Tests are in `test/datascript/test/` with `.cljc` files
- CI runs on GitHub Actions (`.github/workflows/build_test.yml`)
- Tests run against multiple Clojure versions (1.9, 1.10, 1.11.1, 1.12)
