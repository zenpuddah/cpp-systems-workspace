# Agent instructions

Read `OPENCODE_START.md`, `specs/TECHNICAL-CONTRACT.md`, `docs/DELIVERY.md`, `docs/VERIFICATION.md` and `progress.md` before implementation.

## Execute the existing scope

This is an implementation request, not a request for another product brainstorm. Discover existing files, applicable AGENTS instructions, Git/remotes and actual tool/platform capabilities. Preserve existing work and inspect matching repositories before creating duplicates. Write a concise executable plan with concrete files/interfaces/tests, then implement and verify slices. Resolve ordinary design choices yourself; ask only about a blocking credential, destructive operation or material scope conflict.

Default target Git roots are independent siblings: `../cpp-systems-tools` and `../cpp-systems-workbook`. Public product paths inside the tools repository are `projects/tcp-kv/` and `projects/runtime-inspector/`. Never modify unrelated projects or nest repositories under a shared outer Git root.

Deliver both complete verified public tools before the private workbook/companion. Hand over the public result immediately. If hosting is unavailable, a verified local public candidate can be handed over while publication remains pending. Continue the private phase afterward unless scope is narrowed.

## Public/private separation

This public workspace contains only technical specifications and implementation progress. Private source handoffs, personal preparation records, Library links, learner attempts, answer code and oracle implementations must not be copied into it, including through inherited Git history. The private workbook uses independently imported allowlisted snapshots and immutable baselines.

Never automatically push learner attempts or exported AI context. An author-only private area must still be excluded from learner capture/export and attempt-history ancestry. Record immutable assessment identity separately from learner source identity.

## Correctness, evidence and continuity

The detailed technical contract is authoritative over summaries. Keep exact protocol/resource limits, fairness, queue closure, numerical policies and failure tests. Separate implementation results from learner competence. Generated reference code is not evidence of independent learner skill.

Record actual source/test/assessment identities, commands, exit status, OS/ISA/compiler, evidence paths and capability limits. Use passed, failed, unavailable, not run and documented only as distinct states. Failed builds mean tests were not run. Do not invent benchmarks, speedups, Linux results, sanitizer passes or CLion demonstrations. If the IDE is unavailable, its required interactive demonstration remains pending.

Respect configured Git identity/signing/hooks and preserve editing branches/indexes. Use the contract's explicit archive fallback for private attempts when Git recording fails; never invent identities or rewrite user history.

Update `progress.md` and each target repository's `progress.md` after verified slices. Keep one next action and enough state for a later agent to resume. No early unsolicited hints or completion of active learner exercises during independent practice.
