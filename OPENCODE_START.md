# OpenCode starting prompt

Implement the C++ systems tools and subsequent private workbook specified in this repository. Read `AGENTS.md`, the full `specs/TECHNICAL-CONTRACT.md`, the three focused specs, `docs/DELIVERY.md`, `docs/VERIFICATION.md`, `docs/REPOSITORIES.md` and `progress.md`.

This is an execution request. Begin with read-only discovery of existing files/specifications, applicable AGENTS instructions, Git state/remotes, compiler/CMake/Python versions and actual OS/ISA/profiling capabilities. Preserve existing work. Resolve routine implementation choices yourself and document them. Ask only for blocking credentials, destructive actions or material scope conflicts. Do not modify unrelated projects.

Write a concise phased execution plan with exact files, interfaces and checks, then implement verified slices. Do not stop after another roadmap.

Use independent sibling repositories `../cpp-systems-tools` and `../cpp-systems-workbook`, reusing matching existing work where appropriate. The public product paths are `projects/tcp-kv/` and `projects/runtime-inspector/` inside the tools repository. Keep personal study records, learner attempts, hints/answers and private source documents out of public repositories and their Git ancestry.

Use C++20, CMake 3.25+, CTest, POSIX and Python 3.10+ standard library. Linux is the reference platform; detect and label macOS/VM/ISA observations and unavailable tools honestly. Keep dependencies and scope small.

Deliver in this order:

1. Complete the single-owning-thread, nonblocking `poll` TCP key-value server with the exact ASCII protocol, bounded resources/fair turn budgets, RAII ownership, partial-I/O handling, deterministic failure tests and monotonic shutdown.
2. Complete the bundled-specimen Runtime & Performance Inspector with build/link/load/memory reports, equivalent CPU/memory workloads and the separate bounded mutex/CV queue experiment.
3. Verify public presets/core/integration/sanitizer checks as supported, produce diagrams and reproducible results, and investigate one server and one CPU/concurrency performance question. At most two plausible changes per question; no forced speedup.
4. Hand over the verified public result immediately: actual commands/results, source revision, repository locations/checked visibility, evidence files and platform limitations. Hosting unavailability must not block a verified local handoff.
5. Pin the completed reference and derive the separate private workbook: approximately 18 cumulative project exercises, six Q01–Q06 timing/numerical cards and one minimal original DSA reuse sample. Preserve imported-reference and workbook-start baselines; validate the complete author sequence.
6. Build the bounded private `learn.py` CLI, generated read-only HTML dashboard, explicit timers, isolated exact-source attempt history and bounded answer-free context export. Validate CLion actions and the end-to-end workflow when the IDE is available. If it is unavailable, leave that required demonstration pending rather than claiming completion.

The full technical contract defines the exact checks, capture/history boundaries, manifest model, queue semantics, numerical oracles and CLion requirements. Do not expand into an editor, online judge, trading engine, server thread pool or lock-free project. Do not enable automatic learner-history pushes, overwrite baselines or expose reference/oracle answers in learner snapshots/context.

Update this repository's `progress.md` and the target repositories' `progress.md` after every verified slice. Record exact source/test/assessment identity, command, result, limitations and one next action. Keep implementation completion separate from independent learning. Delegate independent tasks only with explicit file ownership. If interrupted, leave exact state and the next command so the next run resumes instead of starting over.

Begin discovery and implementation now.
