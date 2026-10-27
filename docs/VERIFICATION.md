# Verification and handoff evidence

## Evidence states

Use `passed`, `failed`, `unavailable`, `not run` or `documented only` for each check. Store exact command, source/test/assessment identity, environment, exit code or signal, relevant result and evidence path. A failed configure/build means dependent tests are **not run**, not zero passed. Explanatory documentation and actual interactive IDE demonstration have different evidence states.

## Public build matrix

The following presets/commands are targets to implement in the tools repository; they do not exist in this starter repository. Expose matching configure/build/test presets, or document the exact equivalent command if the installed toolchain requires it.

```bash
cmake --preset dev
cmake --build --preset dev
ctest --preset dev --output-on-failure
```

Repeat with separate `release`, `asan` and `ubsan` configurations. Provide a separate supported TSan configuration for concurrent code, without combining it with ASan. Preserve meaningful tests/assertions in release. The builder must report the actual test count and result, not merely a successful shell invocation or empty CTest suite.

| Check | Required evidence |
| --- | --- |
| Linux reference | Actual Linux OS/ISA/compiler, preset/build/test commands and results; otherwise explicit unverified limitation |
| macOS development | Actual supported commands/results or documented-only status; identify Mach-O and tool differences |
| ASan/UBSan | Actual supported run outcomes and excluded opt-in fault demo |
| TSan | Queue configuration/run outcome or precise capability failure; no universal race-freedom claim |
| TCP integration | Ephemeral-port READY handshake, deadlines and unconditional cleanup; deterministic short-write tests |
| Queue | Exact-once plus empty/full/uneven closure, producer/consumer wakeup and joining |
| Inspector | Relevant generated artifacts, independent workload checks and labeled translator/model output |
| Public handoff | Correct quick start, actual sample output, diagrams, test evidence and supported-platform limits |

## Measurements

Keep environment/revision, compiler/flags, workload, seed, repetitions, sample counts, units, warm-up/setup boundaries, checksums, raw samples and errors. Separate client intended issue, actual issue and reply completion where measured; observed round-trip latency is not automatically server service time. Handle zero/invalid elapsed time and fractional rate arithmetic explicitly.

One server question and one CPU/concurrency question are enough. At most two plausible changes per question; verify equivalent correctness before comparing performance. No-change/inconclusive results are valid. Record `perf`, tracing/debugger and counter limitations; do not replace unavailable observations with a desired explanation.

## Private checks

The full contract defines acceptance tests for baselines, full author sequence, exact source execution, dirty/new/deleted and unchanged inputs, runner locks, unstable captures, failure/crash recovery, archive fallback, solution/secret exclusions, timers, changing test revisions and HTML escaping. Record immutable assessment identity even when the oracle is outside snapshots.

Validate actual CLion configuration files against the installed version and official documentation. Mark GUI steps not exercised if the IDE is unavailable; the required demonstration remains pending and the full private acceptance gate is not complete. Hand over useful implemented work with that explicit limit. No claim that native debugging follows a Python-spawned child without a tested mechanism.

## Handoff format

For each phase, report:

1. Repository/local location, checked visibility and exact commit/reference.
2. Actual first-run commands and supported environment.
3. Tests/checks with explicit states and evidence paths.
4. Measurements and supported conclusions, including no-change results.
5. Remaining issues/capability limits and one next action.

Implementation completion and learner evidence remain separate. Update the dated ledger; preserve failed and superseded attempts.

## Required memory extension checks

The exact acceptance cases live in [MEMORY-ALLOCATION.md](../specs/MEMORY-ALLOCATION.md). Verify measured standard-layout offsets/padding/stride; buffer alignment and payload accounting; exact-fit/huge/invalid/exhausted allocations preserving state; raw storage and nontrivial lifetime counters; fixed-slot reuse and safe invalid-release rejection; PMR hard-cap/fallback/resource ownership; comparable costs and correctly labeled fragmentation/retention metrics.

Use deterministic tests and relevant existing regressions in supported dev/release/ASan/UBSan configurations. Run the extended private author chain and capture/export-exclusion regressions through the current companion. No stale-pointer dereference or invalid standard-resource deallocation is an acceptable negative test. Record source/test/assessment identities, actual evidence and unavailable platforms. Preserve old release and starter pins; new tasks/extension tests need distinct versions. Specification publication is not an implementation pass or independent learner gap closure.

