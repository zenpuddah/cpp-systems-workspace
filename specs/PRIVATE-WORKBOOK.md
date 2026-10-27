# Private workbook and companion

This phase follows delivery of both verified public tools. The full [implementation contract](TECHNICAL-CONTRACT.md) defines the required details; this file is a navigation checklist.

## Reconstruction

Pin the public revision in `reference.lock.json`. Import an allowlisted reference snapshot into independent private history; preserve an untouched imported-reference baseline and a distinct `workbook-start` baseline. Prepare all build setup, drivers, diagrams and source files before learner use. Learner edits accumulate rather than being replaced by the next exercise.

Approximately 18 focused core exercises cover meaningful implementation gaps, debugging, safe historical C++ modernization and focused design refactors. Missing behavior must compile and fail explicitly, without UB or fake success. Each task needs stable identity, prerequisites, exact test/contract version, editable paths, checks, three hint levels, an author reference and a transfer variation. Validate the entire author solution sequence against product behavior.

Keep reference solutions/oracle implementations outside learner capture, exports and attempt-history ancestry. They may exist in an author-only private area; private visibility alone does not make leaking an answer acceptable.

## Required memory tasks M01–M05

Add the five tasks specified in [MEMORY-ALLOCATION.md](MEMORY-ALLOCATION.md): M01 layout/alignment/padding; M02 caller-buffer bump allocation and lifetime; M03 fixed-slot individual reclamation; M04 std::pmr resources, fallback and lifetimes; M05 cost/fragmentation investigation. Preserve the original core/Q/DSA catalog IDs, task/test versions and learner work. These memory tasks are additional requirements, not replacements or evidence of learner mastery.

Reuse the current manifests, immutable source/history/assessment pipeline, hints and transfer checks. Validate author solutions separately. Keep original imported-reference/workbook-start pins intact; derive a distinct versioned extension starter/reference from the completed new public extension. Do not reset live learner files to either an author solution or the new starter. Verify the compatible prerequisite state for joining the extension.

## Numerical cards

Add these six small private C++20 cards using the same harness and history:

| ID | Contract focus |
| --- | --- |
| Q01 | Clocks/time points/durations; steady elapsed time; integral/fractional and negative unit conversions; injected clocks |
| Q02 | Explicit epoch/integer ns, checked differences and 100-ms half-open buckets; negative floor and ties; premature-double precision loss |
| Q03 | Actual floating properties, round trips, magnitude spacing, numeric_limits/nextafter, nonfinite/signed-zero handling where supported |
| Q04 | Justified exact or absolute/relative comparison, nonfinite and tick/extra-decimal rounding policy; no approximate ordered comparator |
| Q05 | Naive/compensated sums, cancellation and no-improvement cases; independent Decimal/Fraction oracle with declared input meaning/precision |
| Q06 | Checked integer money/ticks/fees; buy 3 at 100.10 then sell at 100.15, 0.01 fee per transaction per side: 15 cents gross, 13 net |

The full contract supplies the edge cases and oracle boundaries. No silent fast-math, incidental last-digit contract or inference of broad quant expertise from green tests.

## Companion boundaries

Python standard library, one `learn.py` entry point, generated read-only HTML with inline assets/data and CLion run actions. No web backend, account system, React stack or editor plugin. Generic task manifests distinguish cumulative project edits from independent problem targets. Add one small original/permitted DSA adapter after the public delivery and working private pipeline.

The wrapper owns configure/build/run/check, so failed builds are recorded and tests are marked not-run. Ordinary native/gutter/terminal runs are not automatically captured. Support next, check current/all, run scenario, notes, explicit session start/pause/resume/stop, dashboard and context export as described in the full contract.

## Durable attempts

Use a locked runner, isolated tool-owned history/index, allowlisted source and an orphan-root `learning/attempts` lineage. Source checkpoint precedes execution; execute that checkpoint, not live edits. Record final results atomically before a result commit. Preserve failed, unchanged, interrupted and unknown attempts. Retry unstable source capture at most three times. Keep baselines, editing branch/index and unrelated files intact. No automatic push or rewrite.

If Git recording is unavailable, execute an immutable checksummed archive and record explicit archive/result identities; do not invent commit IDs. Keep large logs/build caches outside Git with documented retention and truncation.

## Feedback and assessment

Display actual attempt/source/test identities, commands, status, concrete failures, comparable changes and evidence-backed milestones. Track active study time separately from build/test and wall time. Assistance, explanation, correctness, fluency, complexity, transfer and delayed recall remain separate. Error classifications carry provenance and uncertainty. Do not infer mastery, independence or focused time from a passing test or a compiler error.

Export bounded private Markdown/JSON context with relevant learner edits, source/result IDs, outcomes, help, timer notes, unresolved question and one next action. Exclude answers, secrets and unrelated material. HTML must escape diagnostics and source text safely.

## Acceptance checklist

- [ ] Untouched starters build; pending behavior remains explicit; intended bugs fail the expected checks.
- [ ] Full author solution chain passes and preserves cumulative edits.
- [ ] Snapshot identities are exact; dirty/new/deleted files and unchanged runs are preserved.
- [ ] Runner lock, crash recovery, source drift, archive fallback and editing-index/baseline preservation checked.
- [ ] Solution/secret exclusions and bounded answer-free exports checked, including ancestry.
- [ ] Dashboard/context refresh after failed configure/build/test and changing test revisions checked.
- [ ] Explicit timers, assistance provenance and original-task adapter reuse checked.
- [ ] CLion configuration validated and the actual IDE workflow demonstrated. If the IDE is unavailable, record that limit and leave the demonstration acceptance item pending.

The full contract's required end-to-end demo is: select a lesson → tracked failure → inspect feedback/history → learner edit → tracked check → compare → export context → resume. Do not claim an IDE demo from documentation alone.

