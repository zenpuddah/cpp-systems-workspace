# Runtime & Performance Inspector specification

Derived checklist; read the full [implementation contract](TECHNICAL-CONTRACT.md). One useful tool runs/inspects its bundled specimen and produces interpretable reports. No arbitrary-process debugger, full executable parser or production profiler.

## Specimen and inspection

One specimen across two translation units with global, automatic and dynamic objects. Connect these observable stages:

| Area | Required output/evidence |
| --- | --- |
| Build | Preprocessed source; assembly/object output; actual compiler, flags and ISA |
| Link | Defined/undefined symbols, relocations, controlled link failure, static archive and shared-library example |
| Executable/startup | ELF sections versus loadable segments, dependencies and loader/startup explanation |
| Process memory | Maps, permissions, virtual versus physical addresses, object layout/alignment and vector invalidation |
| Translation/faults | Clearly labeled model of pages/frames/MMU/TLB; ordinary page faults, permissions and copy-on-write; historical segmentation distinguished |
| Calls | Annotated actual function-call/ABI/disassembly observation |
| Platform comparison | Native Linux results labeled; Mach-O/macOS observations labeled; Windows PE/DLL documented separately |

`PGM` remains unresolved. Intentional protection failures, if provided, are opt-in disposable-child demonstrations outside normal and sanitizer suites.

## Workloads and concurrency

Compare AoS/SoA and shared/adjacent/spaced/local aggregation with equivalent defined work and independent result checks. Use runtime inputs, defined unsigned arithmetic and observable checksums. Exclude setup/warm-up from measured work according to a declared boundary.

One small bounded producer–consumer queue uses owned items, mutexes and condition-variable predicates. Its contract includes closed state, rejecting pushes after closure, draining accepted items, waking blocked waiters and joining workers. Exact-once delivery and bounded shutdown must hold for empty, full and uneven workloads. Keep it separate from the TCP server.

## Performance investigation

Baseline → question → evidence → hypothesis → justified change if warranted → correctness → remeasurement. Investigate one CPU/concurrency question, with at most two plausible changes. Record raw samples, seed, workload, revision, machine, flags, timing boundaries, checksums and errors. No mandatory speedup or automatic causal claim about caches/false sharing.

Discover supported timing, tracing, sampling and counter capabilities. An unavailable counter or TSan runtime is a documented limitation, not a pass. A VM's host/guest ISA and counters are recorded explicitly.

## Acceptance checklist

- [ ] One documented specimen-to-report entry point with useful output and errors.
- [ ] Inspectable build/link/load artifacts and a deliberate, diagnosed link failure.
- [ ] Actual object-layout/call/memory observations separated from language guarantees and models.
- [ ] Equivalent workload results checked independently before timing comparisons.
- [ ] Queue exact-once, capacity, closure/drain, blocked-producer/consumer wakeup and worker-join checks.
- [ ] Sanitizer support matrix and actual outcomes, including unsupported cases.
- [ ] One honest CPU/concurrency investigation and reproducible raw data.
- [ ] Public purpose, features, quick start, output, architecture, tests/results and limitations.
