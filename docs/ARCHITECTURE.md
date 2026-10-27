# Architecture boundaries

These diagrams describe the agreed design, not an inspected implementation. Replace proposed component names with actual code references as implementation proceeds.

## Server ownership and byte flow

One event-loop thread owns all connections and the store. A readable event does not mean one complete request, and one write call does not mean a full reply.

```mermaid
flowchart TD
    P["poll readiness and runnable buffered work"] --> R["Bounded read or accept"]
    R --> I["Per-connection unconsumed input"]
    I --> C["Parse complete commands within turn budget"]
    C --> S["Bounded key-value store"]
    S --> O["Per-connection pending output"]
    O --> W["Bounded partial writes"]
    W --> P
    C --> P
```

The real implementation must schedule complete buffered commands even when no new socket event arrives. Output offset/state survives partial writes. Writable interest is active only with pending bytes. EOF and draining have explicit states and a monotonic deadline; a slow connection cannot own the loop indefinitely.

## Inspector: artifacts versus running process

```mermaid
flowchart TD
    S["Two source translation units"] --> O["Assembly and object files"]
    O --> L["Linker and libraries"]
    L --> E["Executable image"]
    E --> D["Loader and startup"]
    D --> M["Process mappings and object layout"]
    M --> W["CPU, memory and queue workloads"]
    O --> R["Inspected report and evidence"]
    E --> R
    M --> R
    W --> R
```

Report what was observed, on which platform and with which commands. Virtual addresses do not identify physical frames. The toy page translator is a model. Disassembly and ABI/layout observations are implementation/platform evidence; they do not redefine C++ language guarantees.

## Private runner boundaries

Keep four areas separate: live learner checkout; author/reference assessment material; isolated allowlisted attempt history; tool-owned execution workspace. Assessment implementation stays outside learner snapshots, but its immutable version/hash must accompany the result so both tested source and assessment are reproducible.

A source checkpoint is created before execution. A final result is persisted atomically before its result commit. Later live edits belong to another attempt. The dashboard and context consume recorded facts; they do not execute native commands or independently infer learner understanding.
