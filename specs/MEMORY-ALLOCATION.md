# Memory layout and allocation extension

Required scope addition, 2026-10-27. This is a bounded extension of the existing Runtime & Performance Inspector and its private workbook. The requirements below are newly specified, not implemented or learned by this documentation update.

## Integration and baseline rules

Read current progress and inspect the actual tools/workbook before making changes. Reuse the completed specimen, build/test presets, reporting/evidence pipeline, task manifests and companion. Keep the existing server and numerical behavior intact. Preserve `public-v1`, imported-reference and workbook-start identities and all learner work/history. Develop the extension on ordinary new branches/commits; pin a new public extension revision and a distinct versioned private extension starter instead of moving old tags or rewriting baselines. Update locks/manifests without making old task/test identities ambiguous.

Public output: small readable reference components, deterministic correctness tests, an Inspector layout/allocation report and measured evidence. Private output: five focused tasks M01–M05 with prepared files, protected assessments, three hint levels, transfer variants and tracked attempts. These are additional tasks; the earlier 18 core + six Q cards + one DSA sample remain intact. Keep author solutions and oracles outside learner capture, exports and attempt ancestry. Use the existing companion; build no new dashboard or allocator framework.

## M01 — Object layout, alignment and padding

- Measure `sizeof`, `alignof`, standard-layout member offsets using `offsetof`, array element stride and supported `alignas` effects. Include two simple structs with the same scalar fields in different declaration orders.
- Report a byte-oriented member/padding map with each member's offset/size, internal gaps and tail padding. Use standard-layout fixtures without inheritance, bit-fields, overlapping members or `[[no_unique_address]]`; gate their use with `std::is_standard_layout_v`. Do not apply portable `offsetof` assertions to arbitrary classes.
- Compute and test the map against actual member extents and object size on the active compiler/ABI/ISA. Do not hard-code universal offsets, a fixed padding total or an assumption that reordering always saves bytes. Distinguish member alignment, allocation alignment, object padding and cache-line spacing.
- Treat padding as layout space, not initialized meaningful data. No dependence on padding-byte contents, raw struct serialization or `memcmp` as semantic equality. Document limits of a memory image and why a portable wire format needs explicit fields.

Acceptance: fixture fields and sizes are unchanged between reorderings; maps account for every byte of the simple fixture; stride matches actual object size; alignment is satisfied; platform-specific output is labeled. Explain one observed internal gap and the reason for tail padding.

## M02 — Allocate from a caller-owned buffer

Implement a small single-threaded raw-storage bump arena accepting a non-owning byte-buffer view. The caller owns the buffer and keeps it alive. Expose an operation equivalent to `allocate(bytes, alignment)` plus `reset`, capacity/consumed/remaining counters; define its exact interface before implementation.

Required raw API contract:

- Allocate variable byte sizes with a declared supported power-of-two alignment range. Reject zero-byte requests, zero/non-power-of-two/unsupported alignment and exhaustion with `nullptr`, without changing arena state. These are this exercise's policies, not claims about all C++ allocators.
- Correctly handle an initially unaligned buffer view, alignment padding, exact fits, one-byte-over-capacity cases, over-aligned fixtures within supported capabilities and repeated mixed size/alignment requests. Account for payload as well as alignment padding; `std::align` alone does not consume payload bytes.
- Check remaining capacity before arithmetic/pointer advancement so huge requests cannot wrap. Successful blocks are aligned, non-overlapping and wholly inside the supplied buffer. No hidden heap/upstream allocation or buffer ownership transfer. A failed request preserves earlier allocations and counters.
- No individual block reclamation. `reset()` reclaims storage collectively and invalidates prior allocation handles; never use those handles afterward. Explain this limitation explicitly rather than presenting it as arbitrary-size allocate/free.
- Demonstrate storage versus object lifetime with correct construction/destruction of a nontrivial counted type, including a throwing constructor. `reset` does not run destructors: destroy live objects before reset/reuse. If typed helpers are added, specify their exception policy without unsafe rewinds across nested allocations. Do not use casts or `std::launder` as a substitute for required construction/lifetime management.

Acceptance: deterministic mixed-allocation trace, invalid/huge/exhausted requests leave state unchanged, alignment/padding counters are correct, live blocks never overlap, supported over-alignment and unaligned start pass, reset/reuse is checked without dereferencing stale storage, and destructor counts validate the declared lifetime policy.

## M03 — Fixed-size pool with individual deallocation

Implement one bounded fixed-block pool using a declared slot payload size/alignment and a free-slot list. This adds individual reclamation for fixed-size slots; it is not a general variable-size malloc/free implementation.

- Declare aligned slot stride, usable slot count and any metadata storage/overhead. Account for metadata and round-up bytes. Allocate the pool from bounded caller-provided storage; no heap allocation on allocate/deallocate paths.
- Return exhausted failure predictably, reuse released slots and keep live slots disjoint. Raw-storage lifetime responsibilities remain with the caller unless a typed wrapper explicitly owns them.
- Define and test rejection of foreign pointers, interior pointers and double release before corrupting the free structure. Validation must not subtract unrelated pointers or dereference an untrusted pointer. Bounds and metadata-index arithmetic are checked.
- Use deterministic full/empty, varied release order and repeated reuse tests with no lost/duplicate slots. Document single-threaded use and validation cost separately from the normal free-list operations.

Acceptance: allocate all slots, reject exhaustion, release/reallocate and verify expected reuse, reject the defined invalid releases without state change, and reconcile live/free slot counts and metadata/padding accounting. Explain what changes if variable-size individual deallocation becomes an interview requirement.

## M04 — Use std::pmr with explicit resource lifetimes

- Demonstrate `std::pmr::vector` and `std::pmr::string` with a supplied-buffer `std::pmr::monotonic_buffer_resource`, plus a bounded example of `std::pmr::unsynchronized_pool_resource` for repeated allocations. Treat the standard pool resource's multi-size behavior separately from the custom fixed-slot pool.
- Use explicit front-end request counters and a counting upstream resource to distinguish client allocation/deallocation requests from actual upstream calls, bytes/alignments and upstream peak live bytes. The upstream counter cannot observe requests satisfied entirely from the initial buffer. A counting resource is evidence tooling, not a global allocator replacement.
- Show both explicit upstream fallback and a hard-cap monotonic run using `std::pmr::null_memory_resource()`. Buffer exhaustion must be observed as `std::bad_alloc` in the hard-cap case; do not silently spill onto the heap. Pool metadata may itself require upstream allocations, so do not promise it operates on a user buffer merely because a monotonic resource can.
- Show that monotonic per-block deallocation does not reclaim individual storage; release is collective and does not invoke client object destructors. Release invalidates allocations even when their containers still hold capacity. Destroy/reconstruct containers and all objects as appropriate before releasing/reusing a resource. `clear()` alone is not a general permission to release its backing storage and keep using that container.
- Ensure resources, upstream resources and supplied buffers outlive dependent containers/allocations. Document the selected resource on copy/move operations being exercised; do not assume PMR containers always propagate or switch resources. No changes to the process-wide default resource are necessary.
- Record actual library/platform behavior without hard-coded pool chunk sizes, growth factors, upstream call counts or a guaranteed upstream deallocation on each pool free. Destroy/release resources before checking final accounting for retained chunks.
- Match every standard-resource deallocation with its correct compatible resource, original byte count and alignment. Its preconditions are different from the custom pool's checked invalid-release API; do not deliberately violate them in negative tests. Construct fresh resources for independent trials rather than assuming an implementation-specific post-release cursor/initial-buffer state.

Acceptance: container results agree with independent fixtures; allocation counters explain the tested resource route; hard-cap failure and fallback are observed; lifetime/destructor counters and final upstream accounting are correct. Avoid intentionally executing a dangling-resource or use-after-release example; explain the invalid variant separately.

## M05 — Costs, retention and fragmentation

Extend the Inspector's memory report with one small controlled investigation. Compare equivalent allocation workloads/lifecycles using a default new/delete-backed resource, the custom arena, the fixed pool where sizes are compatible, and the selected PMR resources. Declare unsupported comparison shapes rather than forcing them through the wrong allocator.

- Keep operation counts, payload/checksum, resource identity, trace/seed, environment, toolchain/flags, repetitions, raw samples and units. Separate construction/destruction from raw storage allocation/deallocation where possible; report arena reset and setup/warm-up separately. Validate that the measured allocation work is not optimized away. No fixed speedup requirement or claim that a custom allocator is always faster.
- Report requested live payload, backing-buffer capacity and resource-acquired bytes, consumed storage, alignment padding, allocated-slot size rounding, allocator metadata and retained-but-not-live memory as distinct quantities. Define each counter. The host allocator's hidden internals and process RSS are not substitutes for those measured/model counters.
- Explain internal versus external fragmentation and temporal retention. Use a tiny deterministic variable-size free-interval **model** to exhibit sufficient total free bytes but an insufficient largest contiguous block. Label it as a model, not a reconstructed libc heap. Include an optional explicit coalescing step within that same model to show adjacent-hole merging; no production variable-size allocator is required.
- For fixed slots, quantify internal rounding loss and whether a request is too large for a slot; do not label every unused free slot external fragmentation. For bump/monotonic resources, explain retained dead allocations until reset and padding/capacity waste without claiming individual frees reclaim space.

Acceptance: equivalent supported workloads produce correct results; raw timing and storage counters have declared meanings; at least one deterministic fragmented-versus-contiguous trace verifies total-free and largest-free-block calculations. Conclusions distinguish observations, model behavior and implementation-specific host allocator behavior.

## Evidence and learning closure

Run focused deterministic tests and relevant existing regressions in dev/release and supported ASan/UBSan configurations. Do not change production server allocation merely to showcase the new experiments. Preserve existing numerical/concurrency checks; update actual public reports, task manifests, prerequisites, versioned test identities and progress evidence. Thread-safe allocator implementation, global operator-new replacement, general coalescing heap, lock-free allocators and allocator-traits metaprogramming remain outside this bounded pass.

For each M task record implementation/assessment availability separately from learner correctness, explanation, assistance, transfer and delayed recall. Initially the new learner checks are **not assessed**, not failed or mastered. Gap closure requires the learner to independently implement the buffer allocator, explain padding/alignment/lifetime choices, diagnose exhaustion/fragmentation and select a suitable reclamation/resource policy on a changed case. Keep unanswered checks pending and the existing CLion interactive acceptance gate visible.

## Primary C++20 references

- [N4861 sizes, alignments and offsets](https://timsong-cpp.github.io/cppwp/n4861/support.types.layout)
- [N4861 sizeof](https://timsong-cpp.github.io/cppwp/n4861/expr.sizeof)
- [N4861 alignment](https://timsong-cpp.github.io/cppwp/n4861/basic.align)
- [N4861 std::align](https://timsong-cpp.github.io/cppwp/n4861/ptr.align)
- [N4861 monotonic buffer resource](https://timsong-cpp.github.io/cppwp/n4861/mem.res.monotonic.buffer)
- [N4861 pool resources](https://timsong-cpp.github.io/cppwp/n4861/mem.res.pool)

Use the C++20 contract and actual implementation evidence; do not silently adopt newer language/library features.
