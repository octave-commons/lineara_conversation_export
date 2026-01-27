# Events and replay

Created with the assistance of an AI.

## `events.ednlog`

- Append-only
- One EDN map per line
- Designed for tailing + indexing

### Common fields

```clojure
{:event/id     "evt-01H..."
 :trace/id     "trc-01H..."
 :parent/id    "evt-01H..."     ;; optional
 :ts/utc       "2026-01-26T20:33:12.123Z"
 :ts/mono-ns   1234567890123
 :kind         :run/started
 :artifact     {:ir "sha256:..." :wasm "sha256:..." :docs "sha256:..."} ;; optional
 :payload      {...}}
```

## Capability calls (determinism spine)

Every effect produces a call/return pair.

### Call

```clojure
{:kind :cap/call
 :payload {:cap :fs
           :call :read-text
           :args {:path "specs/foo.md"}
           :args/hash "sha256:..."
           :replay/key "rpk:sha256:..."
           :mode :real}}      ;; :real | :virtual | :replay | :blocked
```

### Return

```clojure
{:kind :cap/return
 :payload {:replay/key "rpk:sha256:..."
           :ok true
           :value {:type :string :value "..."}
           :value/hash "sha256:..."
           :dur/ms 3.2}}
```

## Replay key scheme

Replay key should depend only on semantic request:

- `cap`
- `call`
- canonicalized `args`
- `capability_policy_hash`
- optional `world/salt`

\[
rpk = sha256(canonical\_edn({cap,call,args,policy,world}))
\]

## Blob storage

Store big payloads separately:

- `.lineara/runs/<run>/blobs/<sha256>.bin`

Log references only the hashes.

#replay #logging #determinism
