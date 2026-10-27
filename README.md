# C++ systems workspace

Implementation specifications and agent entry point for two small C++20 tools:

1. **TCP Key-Value Server:** bounded nonblocking TCP service with explicit connection state, robust partial I/O and reproducible measurements.
2. **C++ Runtime & Performance Inspector:** a specimen-to-report tool connecting build/link/load artifacts, memory layout, CPU workloads and a small producer–consumer experiment.

**Current status:** [progress.md](progress.md) records the original public release and private workbook/companion results, with an interactive IDE acceptance gate still pending. The newly required [memory layout and allocator extension](specs/MEMORY-ALLOCATION.md) is specified and awaiting implementation/learner assessment; this requirements update does not rerun or reassert previous test results.

## Start with OpenCode

Clone this repository, start OpenCode in its root and paste:

```text
Read AGENTS.md and OPENCODE_START.md. Execute the technical contract in verified slices. Start with read-only discovery, write a concise file/interface/test plan, then implement. Do not stop after another roadmap. Update progress.md after each verified slice.
```

The complete starting prompt is [OPENCODE_START.md](OPENCODE_START.md). For the already implemented tools/workbook, use [OPENCODE_MEMORY_EXTENSION.md](OPENCODE_MEMORY_EXTENSION.md) to add the required memory topics while preserving existing work.

## Documentation

| File | Purpose |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Agent scope, execution and evidence rules |
| [specs/TECHNICAL-CONTRACT.md](specs/TECHNICAL-CONTRACT.md) | Self-contained detailed implementation contract |
| [specs/TCP-SERVER.md](specs/TCP-SERVER.md) | Protocol, resource budgets and server checks |
| [specs/MEMORY-ALLOCATION.md](specs/MEMORY-ALLOCATION.md) | Required layout, buffer allocators, pool/arena, PMR and cost/fragmentation extension |
| [specs/RUNTIME-INSPECTOR.md](specs/RUNTIME-INSPECTOR.md) | Systems reports, workloads, queue and measurements |
| [specs/PRIVATE-WORKBOOK.md](specs/PRIVATE-WORKBOOK.md) | Later private reconstruction/companion requirements, without personal records or answers |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Mechanism and ownership diagrams |
| [docs/DELIVERY.md](docs/DELIVERY.md) | Ordered delivery slices and phase gates |
| [docs/VERIFICATION.md](docs/VERIFICATION.md) | Test, platform, measurement and handoff evidence |
| [docs/REPOSITORIES.md](docs/REPOSITORIES.md) | Public/private repository boundaries |
| [progress.md](progress.md) | Actual implementation advancement and one next action |

## Boundaries

This public repository contains a technical implementation brief only. Product code is built in a separate sibling `../cpp-systems-tools`. After both tools are verified and handed over, derive a separate **private** `../cpp-systems-workbook` for learner starters, author answers, exercise attempts and the CLI/HTML/CLion companion. Do not publish those private records or inherit their Git history in a public repository.

Use C++20, CMake 3.25+, CTest, POSIX and Python 3.10+ standard library. Linux is the systems reference; label actual macOS, VM, ISA and unavailable-tool results. No paid service, trading engine, full IDE or online judge is required.

