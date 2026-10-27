# Delivery sequence

The existing implementation contract is the scope. This is its execution checklist, not a claim that work has run. After discovery the builder writes the short technical plan with exact files/interfaces/tests inside each target repository, then executes it rather than producing another handoff.

## Phase A — usable public tools

| Slice | Deliverable | Check before recording complete |
| --- | --- | --- |
| A0 Discovery | Inspect existing work, AGENTS instructions, Git/remotes, OS/ISA/toolchain and capabilities; settle routine choices | Exact locations, visibility evidence, capability matrix, concrete file/test plan and one next action |
| A1 Server semantics | Build/presets/CTest plus pure parser and bounded store | Protocol/error precedence, lengths/ASCII, missing keys and full-capacity replacement tests |
| A2 Server I/O | RAII descriptors, nonblocking poll loop and connection/output state | Split/coalesced requests, injected transport failures, pending byte offsets and EOF/tail behavior |
| A3 Server resilience | Fair budgets, backpressure, bounded clients/buffers and shutdown | Buffered work without new arrival, slow clients, all limits, half-close/reset and bounded process cleanup |
| A4 Inspector observations | Two-TU specimen and one run-to-report command | Build/link/load/layout artifacts, controlled link failure, actual platform reports and labeled models |
| A5 CPU and queue | Equivalent runtime workloads and small closed bounded queue | Independent workload checks; queue exact-once, blocked wakeups, reject/drain and worker termination |
| A6 Evidence and release | Presets/sanitizers, documentation, diagrams and two bounded performance investigations | Actual command/results matrix; raw measurements/checksums; public content allowlist; reproducible public handoff |

Each meaningful code slice has behavior-focused tests, the minimal implementation and direct verification; commit with the configured identity/policies when available. Independent work can be delegated after shared interfaces are fixed. Do not make measurements by timing a sanitizer build.

At A6, immediately report the tools' locations, revision, visibility, exact first-run commands, test results, evidence files and limitations. If remote hosting is unavailable, hand over the verified local public candidate and mark publication pending. Continue to the private phase after this handoff without making publication, dashboard completion or learner progress a new prerequisite.

## Phase B — private reconstruction

| Slice | Deliverable | Check before recording complete |
| --- | --- | --- |
| B0 Pin/import | `reference.lock.json`, allowlisted reference snapshot, imported-reference and workbook-start baselines | Exact public identity retained; independent private history and protected baselines |
| B1 Core workbook | Approximately 18 cumulative project exercises with prepared source/builds/diagrams/tests and author material | Untouched starter builds, intended gaps/bugs fail, entire author sequence passes product tests |
| B2 Numerical cards | Q01–Q06 with deterministic checks, declared policies and separate oracles | Card-specific boundaries; checked arithmetic/units; actual floating properties; oracle contract |
| B3 Companion | Generic manifests, CLI, explicit timers, exact-source runner/history, generated HTML and context export | Capture/run identities; failures/unchanged runs; drift/lock/crash/fallback tests; editing state untouched; no solution leakage |
| B4 Reuse and CLion | One small original DSA adapter and supported CLion actions | Same generic pipeline; actual configuration validation and interactive demonstration; if the IDE cannot be exercised, that acceptance item and full B4 completion remain pending |

Interpretation of the existing contract: approximately 18 project exercises **plus** six Q cards and **one** minimal original DSA sample. The one sample supplies the later acceptance requirement for reuse; the broader DSA feature catalog remains deferred. Do not expand the core exercise count merely to turn every sub-bullet into a separate lesson.

## Progress discipline

Update this workspace's [implementation ledger](../progress.md) and each target's `progress.md` after verified slices. A failed step has a concrete result and one next action. Keep learned competence separate from implementation completion. Preserve source, test and assessment identities; record unavailable and not-run checks without converting them to passes.

## Required extension — M01–M05

The authorized [memory allocation extension](../specs/MEMORY-ALLOCATION.md) follows existing completed work. Run the [delta prompt](../OPENCODE_MEMORY_EXTENSION.md), not the original bootstrap again.

| Slice | New deliverable | Required evidence |
| --- | --- | --- |
| M01 | Actual struct layout/alignment/internal-tail padding maps | Portable fixture constraints, measured offsets/sizes/stride and labeled ABI |
| M02 | Caller-buffer bump arena and explicit object lifetimes | Alignment/slack/overflow/exhaustion/state-preservation and construction/destruction tests |
| M03 | Bounded fixed-slot free-list pool | Reclamation/reuse, metadata/stride accounting and safe invalid-free rejection |
| M04 | PMR containers/resources and upstream policies | Counting-resource evidence, hard-cap/fallback, object/resource lifetimes |
| M05 | Allocation-cost investigation and fragmentation model | Equivalent work/raw samples, defined storage metrics, total/largest free-block trace |

Complete/verify the public reference and report extension, then pin a new extension reference and add five prepared private tasks through the existing companion. Preserve public-v1 and all earlier immutable private baselines/task IDs. New implementation/learner statuses start not run/not assessed; the old evidence remains evidence for its original scope. The unresolved CLion interactive gate remains visible.

