**[< Home](README.md)**

# Temporal

## Origins

Temporal began life inside Uber as **Cadence**, an internal workflow orchestration engine. It was later spun out as an independent open-source project and company. Its core premise: express durable workflows directly in code, without manually managing state, retries, or failure recovery.

---

## Core concepts

### Worker
A process that registers with the Temporal server to execute workflows and activities. It continuously polls the server for tasks to run.

### Workflow
The definition of a business process, written as an ordinary function. Temporal guarantees its execution **all the way to completion**, even if the worker crashes or restarts mid-run. Workflow code must be **deterministic** — no direct calls to system clocks, random UUID generators, or other non-deterministic sources.

### Activity
A discrete, potentially non-deterministic unit of work: an API call, a database write, sending an email. Activities are executed by workers with configurable automatic retries on failure.

### Child workflow
A workflow launched from within another workflow. Useful for decomposing complex processes or fanning out work in parallel.

---

## The three communication primitives

To interact with a running workflow from the outside:

| Primitive | Role | Analogy |
|-----------|------|---------|
| **Signal** | Send an event → setter | Fire-and-forget `setState()` |
| **Query** | Read current state → getter | Synchronous `getState()` |
| **Update** | Send and wait for a response → setter + getter | RPC call with acknowledgement |

---

## How it actually works

Temporal does not persist the workflow's in-memory state — it persists its **event history**. On every resumption, the worker **replays** that history to reconstruct the current state. This is why workflow code must be deterministic: the same sequence of events must always produce the same result.

**Commands** — What the workflow requests from the server (schedule an activity, sleep, send a signal…). They are produced during replay.

**Event history** — The immutable source of truth: everything that has happened since the workflow started.

---

## The workflow event loop

The workflow runs on a **single thread** (or coroutine, depending on the SDK) and follows a strict execution order:

```
Signals & Updates
       ↓
  ① Process signals and updates
    (drained in order of receipt)
       ↓
  ② Progress the workflow
    (run code until the next await / suspension point)
       ↓
  ③ Execute queries
    (read state that is now fully up to date)

  └─ Single thread — no concurrency, no race conditions
```

This ordering matters: signals and updates are handled **first** because they can mutate state. The workflow then **advances** until its next suspension point. Queries run **last**, guaranteeing they always read a consistent, up-to-date state.

---

## Why single-threaded?

Temporal ensures the event loop runs **sequentially and deterministically**. No race conditions, no mutexes. The model is close to a Node.js event loop or an Erlang actor: one thing at a time, but non-blocking thanks to awaits that yield control back to the scheduler.

> In short: Temporal lets you write business logic as if everything were synchronous and infallible — the engine takes care of the rest.

## Other usefull link :
- [java](java.md)
- [ai](ai.md)