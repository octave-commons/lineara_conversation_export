# Lineara Contract v0.1

Created with the assistance of an AI.

## Canonical truth

- The canonical artifact is **A-IR** (your intermediate representation).
- Everything else (Clojure macros, Wasm, JS, TS, Common Lisp, etc.) is a projection:
  - a way to **author** A-IR
  - a way to **run** A-IR
  - a way to **pretty-print** A-IR

#contract #determinism #ir

## Determinism boundary

A run is deterministic iff all of these are fixed:

- `ir_hash`
- `runtime_hash` (A-VM / Wasm runtime version)
- `input_bundle_hash`
- `seed` (explicit, plumbed everywhere)
- `capability_policy_hash` (effects allowed + routing)

## Observables are explicit

Programs produce a stream of observable events:

- `return`
- `log` (structured)
- `effect` requests (IO only through capabilities)
- optional: `metrics` / `trace` spans

## Effects go through capabilities

No hidden IO. Every effect is a capability call, for example:

- `{:op :fs/read-text ... :cap :fs}`
- `{:op :net/fetch ... :cap :net}`
- `{:op :clock/now ... :cap :clock}`

Capabilities can be:
- **real** (talk to the world)
- **virtual** (sandboxed for tests)
- **record/replay** (golden determinism)

#capabilities #observables
