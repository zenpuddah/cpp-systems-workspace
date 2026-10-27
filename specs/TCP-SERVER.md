# TCP Key-Value Server specification

Derived checklist; the complete [implementation contract](TECHNICAL-CONTRACT.md) is authoritative. No implementation is supplied in this starter package.

## Public behavior

One owning thread, nonblocking POSIX sockets, `poll`, `std::unordered_map`, move-only RAII file descriptors. Bind IPv4 loopback, default port 5555. Tests bind port 0 and consume `READY <actual-port>` from the server.

LF-terminated persistent printable-ASCII protocol:

| Request | Response |
| --- | --- |
| `SET key value` | `OK` |
| `GET key` | `VALUE value` or `NOT_FOUND` |
| `DEL key` | `DELETED` or `NOT_FOUND` |
| Malformed input | Documented `ERROR reason` |

Keys are 1–64 non-whitespace printable bytes; values are 1–1,024 printable bytes. Spaces after the key separator are preserved. Commands are uppercase. GET/DEL accept no extra arguments. Reject CRLF, control bytes and non-ASCII input. Lines are at most 4,096 bytes including LF. Publish exact deterministic error precedence and recovery behavior as an implementation decision before testing it.

## Bounds

| Resource or turn budget | Contract |
| --- | --- |
| Clients | 32; reject excess connections |
| Keys | 1,024; replacing an existing key remains legal at capacity |
| Unconsumed input | 8 KiB per connection |
| Pending output | 64 KiB per connection |
| Commands per client turn | 16 |
| Received/sent bytes per client turn | 16 KiB each |
| Syscall attempts per client turn | 32 |
| Accept attempts per turn | 8 |
| Draining/shutdown deadline | 2 monotonic seconds |

Connection-buffer overflow may close without a final reply. Define accounting so consumed prefixes do not count as unconsumed input. Keep fairness independent of a peer sending another packet. Only request writable events when output remains pending.

## Required mechanisms

- Explicitly configure accepted sockets; do not assume nonblocking mode is inherited.
- Handle partial reads/writes, EINTR, EAGAIN, SIGPIPE, EOF, half-close and reset.
- Retain unsent bytes and incomplete frames; process complete requests and tail bytes before normal EOF draining according to the documented contract.
- Resume buffered complete commands after a turn budget without needing another network arrival.
- Backpressure and bounded memory prevent a slow peer from blocking other clients or exit.
- Use a monotonic draining deadline and unconditional descriptor/process cleanup.

## Acceptance checklist

- [ ] Parser/store boundary tests, exact errors and full-capacity replacement.
- [ ] Every split point of representative requests; coalesced and pipelined requests.
- [ ] Deterministic injected short-write/EINTR/EAGAIN results with verified offsets.
- [ ] Buffered work beyond one command budget progresses with no new arrivals.
- [ ] Slow-reader, connection/input/output/key limits and fair-client progress.
- [ ] EOF with complete requests and a documented incomplete-tail outcome.
- [ ] Half-close/reset, disconnect/reconnect and bounded shutdown.
- [ ] All process tests have deadlines, ephemeral ports, handshake validation and unconditional cleanup.
- [ ] One reproducible performance investigation with raw samples and correctness recheck.
- [ ] Public quick start, output, protocol, state diagram, tests/results and limitations.

Do not add a thread pool, concurrent map, replication, persistence or trading behavior. The queue experiment belongs in the Inspector.
