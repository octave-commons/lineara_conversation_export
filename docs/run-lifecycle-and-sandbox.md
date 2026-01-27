# Run lifecycle and sandboxing

Created with the assistance of an AI.

## Run status derivation

Statuses:
- unknown, running, blocked, failed, passed, canceled

Rules:
- running if `:run/started` and no terminal event
- passed if `:run/finished {:ok true}` (or tests pass + committed)
- failed if `:run/finished {:ok false}` or fatal event
- blocked if:
  - pending required gate
  - blocked capability call
  - missing replay entry
  - pipeline paused

Terminal precedence:
passed/failed/canceled > blocked > running > unknown

## Gate auto-generation triggers

- spec gate when a spec patch would modify canonical specs
- code gate when exports would modify tracked code
- capability gate when policy blocks an effect
- release gate optionally after pass

#workflow #gates #policy

## Projection runner sandbox envelope (host-side)

- fixed `cwd`
- env allowlist
- stdout/stderr captured into events
- limits: timeout, idle-timeout, max-procs
- policy: fs roots, net deny/allowlist

Example envelope:

```clojure
{:exec/id "exe-01H..."
 :projection/id :shadow
 :cwd "/workspace"
 :env/allow ["PATH" "HOME" "XDG_*" "LINEARA_*" "NODE_OPTIONS"]
 :env/set {"LINEARA_RUN_DIR" ".lineara/runs/<run>"
           "LINEARA_TRACE_ID" "trc-..."
           "LINEARA_POLICY_HASH" "sha256:..."}
 :stdio {:capture true :max-bytes 5000000}
 :limits {:timeout-ms 600000
          :idle-timeout-ms 60000
          :max-procs 8}}
```

#sandbox #runtimes
