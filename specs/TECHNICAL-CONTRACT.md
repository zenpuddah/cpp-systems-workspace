# C++ systems tools — technical implementation contract

Implement the following end to end. Start with read-only discovery, write a concise phased implementation plan with exact files/interfaces/checks, then execute it in verified slices. Resolve routine choices yourself and continue; ask only for a genuinely missing credential, a destructive operation, or a material scope conflict. Do not stop after producing another plan.

## Purpose, repositories and delivery order

Build TWO SMALL, COMPLETE PUBLIC SOFTWARE PROJECTS. The lab/puzzle method is private, for reconstructing the code afterward. Build usable tools with clear commands and outputs, rather than a public course or collection of incomplete exercises.

Use one public repository, provisionally `cpp-systems-tools`, containing:

1. **TCP Key-Value Server:** a bounded in-memory TCP service with explicit connection state, failure handling and reproducible measurements.
2. **C++ Runtime & Performance Inspector:** a small developer tool that inspects its bundled C++ specimen and produces CPU/memory/concurrency workload reports. Give it one clear run-to-report workflow. Do not expand it into an arbitrary-process debugger, custom executable parser or production profiler.

After those are completed and verified, derive a separate PRIVATE repository, `cpp-systems-workbook`, containing their prepared learner versions, historical/flawed starters, diagrams, tests, hints, author solutions and the progress mechanism below. Its remote must remain private. Keep personal study records, learner attempts, author solutions and assessment implementations private; they do not belong in this public workspace.

Inspect existing directories, Git state, AGENTS.md and any supplied specifications first. Preserve existing work; use isolated worktrees where necessary. Do not modify unrelated projects. If repository hosting is unavailable, finish the local deliverables and clearly report the remaining publishing step. Do not claim a remote exists or is public/private without checking it.

This technical contract is self-contained. No private source documents or conversation access are required.

**Deliver the public release before building the private workbook/dashboard.** Neither workbook completion nor my learning progress blocks the public handoff. Do not send applications or messages on my behalf.

## Technical scope and verification

Use C++20, CMake 3.25+, CTest, common POSIX interfaces and Python 3.10+ standard-library scripts. Public project paths: `projects/tcp-kv/` and `projects/runtime-inspector/`. Avoid third-party runtime services. Linux is the systems reference environment; detect actual OS/ISA/toolchain. Support macOS development where feasible, explicitly labeling Mach-O/macOS observations. Document a Linux toolchain/VM route for a Mac user without claiming that Linux-only checks ran natively on macOS. No new paid infrastructure is required by default.

Provide separate `dev`, `release`, `asan` and `ubsan` presets; a separate supported TSan configuration for concurrent code; real registered CTest tests; meaningful assertions that remain enabled in release. Intentional memory-protection failures run only in an opt-in disposable child-process demonstration, outside normal/sanitizer suites. Tool/counter limitations are visible, reasoned statuses, not fabricated results.

### Project one

Use one nonblocking `poll` event loop, one owning thread and `std::unordered_map`. Bind IPv4 loopback, default port 5555; tests use port 0 and a `READY <actual-port>` handshake.

Persistent, LF-terminated printable-ASCII protocol:

- `SET key value` -> `OK`
- `GET key` -> `VALUE value` or `NOT_FOUND`
- `DEL key` -> `DELETED` or `NOT_FOUND`
- Malformed input -> documented `ERROR reason` response.

Keys: 1–64 non-whitespace printable bytes. Values: 1–1,024 printable bytes; spaces after the key separator are preserved. Commands uppercase; GET/DEL have no extra arguments; reject CRLF/control/non-ASCII input. Lines at most 4,096 bytes including LF. Publish deterministic error precedence and recovery behaviour.

Limits: 32 clients, 1,024 keys, 8 KiB unconsumed input and 64 KiB pending output per connection. Replacement remains legal at full key capacity. Reject excess connections; connection-buffer overflow can close without a final reply. Move-only RAII descriptors, explicit accepted-socket setup, partial I/O, EINTR/EAGAIN, SIGPIPE, EOF and reset handling are required.

Bound each client's turn: 16 commands, 16 KiB received/sent and 32 syscall attempts; bound accepts to eight attempts/turn. Resume buffered commands without needing more network input. Request writable events only with pending output. Handle tail bytes and complete requests before normal EOF draining. Bound draining/shutdown to two monotonic seconds; a slow peer cannot block exit or other clients.

Test parsing/store boundaries, every split point, coalescing, short writes via injected transport results, half-close, slow peers, fair scheduling, disconnect/reconnect and shutdown. All process tests have deadlines and unconditional cleanup. Do not depend on localhost accidentally producing a short write.

### Project two

Keep one two-translation-unit specimen with a global, local and dynamic allocation. Cover preprocessing, assembly/object output, symbols/relocation/link failures, static archive/shared library, ELF sections versus loadable segments, loading/startup, process maps, virtual versus physical addresses, historical segmentation versus paging, MMU/TLB, ordinary page faults, permissions and copy-on-write. Label a toy address translator as a model. Keep Windows PE/DLL comparison documented separately from native verification. `PGM` remains unresolved, not a guessed acronym.

Provide useful reports and inspected outputs: actual build flags/ISA/ABI; function-call/disassembly trace; object layout/alignment and vector invalidation observations; AoS/SoA and shared/adjacent/spaced/local aggregation comparisons. Validate equivalent results with independent fixtures/reference sums. Use runtime input and defined unsigned arithmetic; do not time setup or accidentally eliminated work.

Include ONE SMALL bounded producer–consumer queue experiment: mutex, condition-variable predicates, owned items, rejected pushes after closure, draining accepted items, waking blocked waiters and joining workers. Prove exact-once delivery and bounded shutdown with empty/full/uneven workloads. Keep this experiment separate from the server; no server thread pool or concurrent-map architecture is needed.

### Investigation and quality

Keep raw measurements with workload, revision, machine, compiler/flags, seed, repetitions, timing boundaries, checksums and errors. Separate test/sanitizer runs from timings. Report throughput/latency distribution with sample counts; distinguish client scheduling delay, observed round-trip latency and unavailable server service time.

Investigate one server question and one CPU/concurrency question using supported timing/tracing/profiling tools. Discover `perf stat`, `perf record/report`, `strace` and debugger capabilities; record unavailable features honestly. Baseline -> question -> evidence -> hypothesis -> justified change, if any -> correctness -> remeasure. Try at most two plausible changes per question; accept no-change/inconclusive conclusions. Never degrade the public implementation to manufacture an optimization story.

No mandatory speedup, kernel bypass, FPGA, DPDK, lock-free queue, full FIX, replication, live trading, GUI framework or new strategy engine. Advanced offered-load/UDP/epoll/SIMD/publication examples can remain private follow-ups.

Public README format: purpose, features, quick start, example output, architecture, tests/results, limitations. Public architecture diagrams explain the tools; private diagrams also guide reconstruction. Favor small readable components and a compact documentation set.

## Private guided implementation

Pin the completed public release; import an allowlisted snapshot into independent private repository history. Record the original public commit in `reference.lock.json`. Preserve an untouched imported-reference baseline and a `workbook-start` starter baseline in the private repository; their private commit IDs may differ from the original public commit; never reset or rewrite them automatically. Provide all files/build setup/drivers/diagrams in advance. Leave meaningful logic gaps, diagnose-and-fix bugs, safe older-C++ modernization and focused design refactors. Learner edits accumulate into both working projects; a later exercise must not replace an earlier answer.

Use approximately 18 focused core exercises, splitting only when a distinct concept needs its own attempt. Each has a manifest ID, prerequisites, protected behavioral contract/test revision, editable regions, check target, reference location, three hint levels and a transfer variation. Validate the author's entire solution sequence against the same product tests before delivery. Missing behaviour compiles and fails explicitly; do not insert fake success, undefined behaviour or throwing destructor/noexcept placeholders.

Tutor method: one concept at a time; let me state objects/state, invariant and transition before code. No unsolicited hints or automatic solutions. Exercise the reference/value, copy/move, lifetime, STL/invalidation and timing boundaries in real project contexts. Passing code, assisted learning and independent delayed recall are separate statuses.

## Generic idea: a training-oriented IDE/workspace

Treat the private mechanism as the first version of a reusable **training workspace**: an IDE companion that retains the evolution of a person's work, feedback and learning evidence across sessions. The two guided systems projects are its first users. Later it can support DSA/LeetCode-style problems, debugging, refactoring, C++ concept checks and performance investigations using the same attempt/history/context model.

This is a product direction and a small extensibility boundary, not permission to build a third large application now. Start as a CLI/HTML companion to CLion, not a new editor or custom IDE plugin. Make the core independent of these project names, Git hashes and exercise IDs; do not hard-code E06 or TCP-specific scoring into the dashboard.

Keep a compact versioned task manifest: stable task ID, task kind, concept tags, prerequisites, baseline/reference identity, owned editable paths, build/check/scenario commands, expected results or rubric, hint levels, retry settings and evidence fields. Use one generic attempt record for source identity, task/test version, command, time, outcomes, assistance and reasoning evidence. Give each task an explicit workspace policy: cumulative project edits versus an independent problem target. Do not force an isolated DSA problem to reset or share the cumulative project source tree; retain its own attempt lineage and files. A small built-in adapter boundary can prepare a task, supply commands, normalize results and contribute context; do not add a plugin marketplace or framework.

Separate evidence dimensions rather than collapsing them into one score: functional correctness, explanation quality, implementation fluency, resource/complexity reasoning, transfer and delayed unaided recall. Passing a test is an observed result; conceptual review and independence require an explicitly attributed assessment. Keep elapsed/active time, familiarity and hints visible so speed comparisons are meaningful.

The normal task loop is: choose one task -> predict/plan -> attempt -> check -> inspect a concrete failure -> revise -> attempt again -> record explanation -> revisit later. Support continuing a partial attempt, pausing a session and taking a later cold pass without losing the original code or rewriting history.

## LeetCode-style sophistication: bounded extension path

After public delivery and a working private project tracker, add ONE SMALL local DSA adapter or demonstration if it fits the remaining scope. A problem is a task with a prepared C++ harness, input/output contract, fixtures and a problem URL/metadata when supplied. Use original or explicitly permitted statements/test data; do not depend on scraping, account login, premium material or automated submission. External verdicts can be recorded manually with provenance. Do not promise a connection to LeetCode's private judging system.

Design the generic model to support these extensions, but do not implement the whole list in the first release:

- **Pattern recall:** offer one problem at a time; in mock mode hide the suggested pattern and hints until the user asks. Record prior exposure as declared/unknown rather than claiming a problem is objectively unseen.
- **Reasoning worksheet:** keep goal/contract, state, invariant, transition, traversal and result separate. Make the worksheet optional and learner-entered, with brief notes sufficient; never prefill the algorithm during an independent attempt. Store revisions so confusion between an invariant and a transition can be reviewed using the learner's own trace.
- **Implementation-mechanics drills:** short focused tasks for indexing/bounds, forward versus reverse indices, half-open ranges, STL operations, comparators and ownership. Keep algorithm-understood/implementation-error as a tutor-reviewed distinction, not an automatic inference from a compiler message.
- **Failure investigation:** display the failing input, expected and observed result and relevant source diff. Later support small differential/property-based test generators and counterexample reduction where a trustworthy oracle exists. Reference solutions and oracle implementations stay in author-only code, not learner snapshots or context; run them at a separate assessment boundary and expose results/counterexamples, not answers. Preserve verified minimized failures as regression cases. These are trusted local practice runs with deadlines/resource controls, not a secure sandbox for hostile code.
- **Adaptive retry queue:** suggest a single next task using unresolved errors, declared help, uncertainty and time since the last attempt. Show the saved evidence behind a suggestion and permit override; do not create an ever-growing compulsory schedule. A cold pass records its explicitly chosen fresh starter and assistance status while preserving all earlier solutions; never overwrite active learner work to reset it. Distinguish same-session correction from a delayed cold pass and avoid repeated drills once fresh variants demonstrate the target skill.
- **Transfer variants:** duplicates, empty/boundary inputs, ordering changes or altered constraints, chosen for the actual contract. Don't label arbitrary mutation as a valid variant or reveal its solution prematurely.
- **Complexity investigation:** compare operation counts or controlled scaling across input sizes; label observations and theoretical arguments separately. A few timing samples cannot certify Big-O or an online-judge acceptance guarantee.
- **Progress feedback:** a concept map, comparable attempt timelines, recurring confirmed errors, first independent pass and successful transfer milestones. Preserve regressions and unknown statuses. Avoid an intelligence/mastery score inferred from raw runtime or green tests.

Use an extendable error taxonomy: algorithm choice, state definition, invariant, transition, indexing/bounds, STL/API use, ownership/lifetime, language-rule selection, complexity, missing test and unknown. Store author/tutor/user attribution and uncertainty for every classification. An AI suggestion must not silently become a confirmed personal weakness.

Require one small proof of reuse: add an original DSA-style sample task through the task manifest/adapter without changing generic history, timers, dashboard or context-export code. The user-facing two-project workbook remains the primary deliverable. A full online judge, editor, contest platform, cloud account system or large problem library is deferred.

## Quant-oriented chrono and numerical training

Add six small C++20 cards to the PRIVATE training catalog. Reuse the existing harness, progress/history, hints and context export; do not build another backtester or modify unrelated projects. Public tools need correct timing/report arithmetic, while these richer quant exercises follow public delivery. Each card has prepared files, deterministic checks, a prediction, one focused implementation/repair and a transfer variation.

- **Q01 — Clocks, durations and units:** distinguish `time_point`/`duration`, monotonic elapsed measurements and wall-clock labels. Use `steady_clock` for durations/deadlines; inspect `high_resolution_clock::is_steady` rather than assuming it is preferable. Compare integral/fractional conversions of 1,500 microseconds to milliseconds, including negative durations, truncation, `floor`, `ceil` and `round`. Inject clocks instead of asserting precise sleep timings.
- **Q02 — Market event timestamps:** use an explicitly defined epoch and integer nanosecond units; compute differences and 100-ms half-open buckets. Include exact boundaries, earlier-than-origin events and equal timestamps with a declared tie/order policy. Use mathematical floor for negative buckets, checked conversion/arithmetic, and demonstrate precision loss from converting large epoch timestamps to `double` before subtraction.
- **Q03 — Floating-point representation:** inspect `float`/`double`, decimal round trips, magnitude-dependent spacing, `numeric_limits`, `nextafter`, finite values, NaN, infinity and signed zero where supported. Record actual type properties; do not assume `long double` adds precision on every platform. Gate exact IEEE-specific assertions on the actual representation.
- **Q04 — Comparisons and rounding:** choose exact equality or justified absolute/relative error according to units and error budget; include near-zero and large/small values, NaN/infinity policies and explicit tick/monetary rounding. Do not teach a universal epsilon or use non-transitive approximate equality as a sorting/map comparator. Specify rejection or rounding of extra decimal places.
- **Q05 — Stable aggregation:** compare naive and compensated summation on synthetic price/return-like inputs, including cancellation and cases with no improvement. Use a bounded independent Python `decimal` or `Fraction` oracle; state whether it represents intended decimal inputs or the exact already-rounded binary inputs. Choose oracle precision explicitly; compensation is not a guarantee for every dataset or parallel bitwise reproducibility.
- **Q06 — Prices, fees and P&L:** parse decimal prices into checked scaled integers and enforce tick size. Toy USD fixture: buy 3 units at 100.10, then sell all 3 at 100.15; fees are 0.01 per transaction on each side, not per unit. Gross profit is 15 cents and net profit 13 cents. Compute these exactly in integer cents, check multiplication/addition/subtraction for overflow, reject invalid/non-finite input, and define the denominator/zero policy for any return. This is a small accounting fixture, not a general financial engine.

Apply the timing lessons to real reports: named units, fractional rates without accidental integer division, explicit zero/invalid elapsed-time handling, warm-up and timing boundaries. Keep internal precision separate from presentation rounding. Correctness exercises do not use `-ffast-math` or hardware-dependent incidental final digits as the contract. Verify language rules against C++20 rather than silently adopting a newer draft.

Extend error tags with clock choice, duration truncation, timestamp unit/overflow, floating comparison, rounding, cancellation and money scaling. Keep confirmed conceptual errors separate from tool suggestions and measurement noise. Do not infer quant expertise from passing six cards. Primary reading pointers: C++20 chrono contracts and David Goldberg's floating-point tutorial.

## Fast feedback: CLI plus a small HTML dashboard

Implement this inside the PRIVATE workbook with Python standard library plus one generated HTML page with inline CSS/JS/data. No React, Node backend, database, authentication or custom CLion plugin. It is supporting tooling, not a third portfolio project.

Provide one `learn.py` entry point with these capabilities:

```
python3 learn.py next
python3 learn.py session start E06
python3 learn.py check E06
python3 learn.py check --all
python3 learn.py run E06 --scenario partial-write
python3 learn.py note E06 --category boundary --text "..."
python3 learn.py session pause
python3 learn.py session resume
python3 learn.py session stop
python3 learn.py dashboard
python3 learn.py context E06
```

These are interface requirements, not commands that already exist. Every configured tracked run/check refreshes dashboard and context, even when configure/build/tests fail. Show an immediate concise terminal card: attempt ID, source hash, build/check status, actual tests run, change from comparable prior attempt, current failure and links to details/dashboard/context. `next` proposes one action without giving away its solution.

Dashboard: project/exercise map, current task, checks passed/remaining, attempt timeline, explicitly timed study sessions, recurring error categories, recent code change summaries and evidence-backed milestones. Let me compare two attempts and filter by lesson. Use restrained colors and small completion feedback when a real check or milestone is earned. No invented mastery percentages, punitive streaks or points merely for rerunning unchanged code. A failed build means tests were not run, not that zero tests passed. Compare counts only across the same relevant test/contract revision.

Keep code-check, explanation, transfer, assistance and delayed-cold-pass statuses distinct. Track assistance as independent/hints/AI-guided/AI-written/unknown, declared by me or a tutor with provenance; never infer independence from tests. Compiler diagnostics can suggest an error category, but distinguish a tool's classification from my confirmed conceptual error. Measure build/test duration and overall elapsed time separately. Focused study time uses explicit start/pause/resume/stop; do not call idle time or a process runtime focused learning. No keystroke, screen or other surveillance.

Generate a read-only HTML report that works as a local file and can be refreshed/opened in CLion/browser. Run commands come from CLI/CLion, not nonfunctional HTML buttons pretending to execute native programs. Avoid a custom web service for this version.

## Every tracked run has durable Git history

Do this only in the private learning repository. Use an ordinary local `learning/attempts` branch in a separate history worktree with its own index. Initialize its history from an orphan root containing only allowlisted learner source and attempt records, not from a workbook branch whose tree/ancestry already contains author solutions. Never use the editing checkout to create or clean that orphan history. Keep my editing worktree, checked-out branch, staged changes and unrelated files untouched. The history branch must be visible in normal Git/CLion history; do not force me to switch branches to study.

For each tracked run/check:

1. Acquire a project-level runner lock. Snapshot an explicit manifest of source, required assets, build configuration and tests, including allowlisted new files and deletions. Verify capture consistency if files change during copying; retry capture at most three times, then record snapshot-unavailable instead of looping while I edit. Exclude credentials, `.env`, solutions, `.git`, caches, binaries and unrelated files. Stage exact paths in the tool-owned history worktree; never `git add .` in my checkout.
2. Create a **source checkpoint commit before execution**, including unique attempt ID, lesson, command, assistance label and source/config/test identity. Repeated identical code still records a distinct attempt. Respect existing Git identity/signing/hooks; do not invent an identity or disable policies. If Git recording is unavailable, preserve an explicitly labelled immutable source archive/hash plus attempt record and explain that a commit was not created. Fallback mode remains executable without Git: extract the archive into the tool-owned runner workspace, retain an append-only result record referencing its archive ID, and export source/result IDs with commit fields explicitly absent. Do not fabricate hashes or silently drop history when a commit fails.
3. In Git mode, build/run the checkpoint in a tool-owned detached runner worktree; in archive fallback mode, execute its verified extraction, so subsequent edits belong to a later attempt. Favor one stable runner path with a compatible incremental build cache over a full fresh rebuild per click; cache identity includes toolchain, preset and build configuration. Only reuse or update this explicitly tool-owned workspace after the prior attempt is secured; verify its source matches the new checkpoint. Run the current lesson target first; the full suite is a separate action. Record any source drift caused during execution. Do not report a live edited tree as the exact committed source tested.
4. Write the final result atomically to a durable local record before attempting its Git commit. Record a **result commit** referencing the source checkpoint (or the append-only archive result in fallback mode), with statuses, exit codes/signals, commands, durations, test results, small bounded diagnostic summaries and observations. Preserve failures, no-change runs, cancellations and interrupted attempts. After a crash, recover an existing final record and retry only its missing recording step; classify a checkpoint without a final record as interrupted/unknown and never invent its exit code.
5. Refresh dashboard/context and print both commit hashes, or clearly labelled archive/result IDs in fallback mode. Maintain baseline and attempt commits; no automatic history rewriting, squash, remote push or forced cleanup. Keep large raw logs/build outputs outside Git, with clear size/retention limits; preserve summaries/hashes and mark truncation. Clean obsolete runner worktrees only after results are secured; preserve compatible build caches within a documented size limit. Never clean the editing/history worktree or unrelated paths.

The history branch has no automatic upstream/push. Local branch names do not provide privacy: verify the enclosing remote is private and document manual synchronization separately. No automatic pushes of learner attempts, AI context or personal records.

## CLion integration

Use supported CMake/CTest and shared project run configurations. Supply `Learn: Check Current`, `Learn: Run Current`, `Learn: Check All`, `Learn: Dashboard` and `Learn: Export AI Context`, plus normal native CMake debug configurations for each tool. A script wrapper must own the full configure/build/check lifecycle to record build failures; a Before Launch hook alone cannot capture final results.

Expose real CTest tests to CLion's native test runner, and print clickable source diagnostics using CLion's supported output-filter format. Use project macros and documented one-time interpreter/toolchain setup, not hard-coded personal paths. An External Tool may require a documented local setup step; do not pretend a repository can silently install IDE-wide settings. Native C++ debugging stays native; do not claim the debugger automatically follows a Python-spawned child.

Label tracked configurations clearly. Arbitrary gutter/debug/terminal launches are not automatically recorded; document this boundary and provide the configured actions as the standard learning workflow. Do not claim an interception mechanism you have not implemented. CLion's recent test history is not the durable progress database. Verify supplied configuration files and document any IDE interaction that could not be exercised here.

## AI continuity without repeated explanation

After each tracked attempt, generate private `current-context.md` and machine-readable JSON. Include project goal, pinned baselines, current lesson/contract, source/result commit IDs, recent relevant diffs, commands and outcomes, declared assistance/hints, explicit session time, confirmed errors versus hypotheses, unresolved questions and one next suggested experiment. Include enough relevant code to reason about the failure, bounded by a documented size cap; link omitted full records by exact attempt/file identity. Include my own edits and reasoning revisions when relevant. Do not include answer patches, credentials or unrelated private data.

Make this resumable by OpenCode or ChatGPT by attaching/pasting the context file; optionally provide a copy command that prints the file path/content. No paid API, credentials, autonomous AI grading, invisible conversation access or automatic external upload is required. Add private AGENTS.md instructing a future tutor to read current context, preserve my edits, avoid unsolicited hints and never silently finish other exercises.

## Acceptance checks and handoff

Public phase: clean builds, meaningful core/integration tests, documented Linux/macOS verification status, bounded resource/failure handling, useful sample outputs, readable diagrams and honest measurements. Hand over the public result immediately when ready.

Private phase: the untouched starter builds; pending tasks remain visible; every intended bug is caught; the complete author solution chain produces both working projects. Prove the runner preserves my branch/index and original baselines, snapshots dirty/new/deleted files consistently, records unchanged/failing/interrupted runs, prevents concurrent recording, respects path exclusions, and runs the exact checkpoint. Test progress calculations across changing test revisions, stale results, pauses/crashes and assistance labels. Test dashboard/context refresh after failures and escape diagnostic text safely in HTML. Verify the original DSA-style sample task uses the same generic attempt/timer/history/reporting pipeline, keeps reasoning fields distinct and does not leak its reference solution.

Deliver one working demonstration of: open CLion -> select a lesson -> tracked failing check -> inspect feedback/history -> make a small change -> tracked check -> view comparison -> export context -> resume with the same state. Do not mark this demonstrated if IDE steps were only documented.

Keep progress.md with completed slices, actual verification commands/results and the next action. At the end provide repository locations/visibility, first run commands, CLion setup steps, example feedback, evidence locations, supported-platform limits and remaining issues. Report implementation completion separately from my learning progress. Begin discovery and carry the work through these deliverables without expanding scope.

## Integration references

Check the installed CLion version against official documentation; these references establish supported integration surfaces, not proof an IDE test was run:

- https://www.jetbrains.com/help/clion/ctest-support.html
- https://www.jetbrains.com/help/clion/run-debug-configuration.html
- https://www.jetbrains.com/help/clion/settings-tools-external-tools.html
- https://www.jetbrains.com/help/clion/editing-html-files.html
- https://git-scm.com/docs/git-worktree
