# OpenCode — memory layout and allocator extension

Continue the existing systems tools and private workbook. Do not rebuild the completed project from scratch or automatically solve a learner's active task.

Read `AGENTS.md`, `progress.md`, `specs/MEMORY-ALLOCATION.md` and the existing technical/Inspector/workbook/verification documents. Inspect actual sibling repositories, their AGENTS files, Git state, evidence, reference locks, manifests and current learner edits before changing anything. The workspace ledger records an existing public release and private implementation; validate the relevant current identities/commands and preserve that work.

The user authorized closing these gaps: internal/tail padding, sizeof/alignof/offsetof, alignment, caller-buffer allocation, individual fixed-slot reclamation, allocation/deallocation costs, fragmentation, pools/arenas and std::pmr. Implement the required M01–M05 extension inside the existing Inspector/reference and private workbook. The detailed contract is `specs/MEMORY-ALLOCATION.md`.

Write a short concrete delta plan, then execute verified slices:

1. Add measured object-layout/padding maps and deterministic layout checks.
2. Add the bounded buffer-backed bump arena and fixed-slot pool reference components with explicit alignment, capacity, failure, reclamation and lifetime tests.
3. Add PMR resource/container examples, upstream counters, hard-cap exhaustion/fallback and correct lifetimes.
4. Add controlled allocation-cost reports and the small labeled fragmentation model, with equivalent work, raw measurements and no forced speedup.
5. Add prepared private M01–M05 tasks, assessments, hints/author material and transfer variants through the existing companion. Validate the author chain and new regression targets without touching live learner answers.

Keep C++20 and the current CMake/CTest/Python stack. Preserve existing public-v1, imported-reference and workbook-start commits/tags plus all old task/test versions. Use ordinary new branches/commits and a distinct pinned extension reference/starter; do not move old baselines or reset the learner's tree/index. Preserve cumulative edits and let a learner join the extension from an explicit compatible prerequisite state. Do not automatically apply author fixes to active attempts.

Keep implementations/tests/public reports in the tools repository; keep learner attempts, hints, author solutions, oracle implementations, context and private lock identities in the checked-private workbook. Publish only the established public allowlist. Preserve source/assessment identity separation and exact-source runner/capture boundaries.

Run focused deterministic and relevant existing regression checks in supported dev/release/ASan/UBSan environments. Report any unrun platform checks honestly. The previous CLion interactive gate remains pending until actually demonstrated. Update public implementation ledgers and private task/learning ledgers separately; new requirements and generated solutions do not establish independent learner mastery.

Resolve routine design choices yourself; ask only about a blocking credential, destructive action or material conflict. Do not add a general malloc implementation, thread-safe allocator, global-new override, new IDE/dashboard, trading feature or unrelated refactor. If interrupted, leave exact changed files, source/test identities, actual results and one next command. Begin discovery and implement this extension now.
