# Review gates and capability policy

Created with the assistance of an AI.

## Review gates

Gates are files in `reviews/` that control pipeline progression.

Example:

```markdown
# gate
kind: spec
trace_id: trc-01H...
proposal_hash: sha256:...
status: pending   # pending | approved | rejected
notes: |
  -
```

Gate types:
- spec
- code
- capability
- release (optional)

#reviews #workflow

## Capability policy (first-class artifact)

Example:

```clojure
{:policy/version 0
 :policy/id "pol-01H..."
 :caps
 {:fs    {:allow true
          :roots ["./specs" "./tasks" "./reports" "./reviews" "./designs"]
          :modes #{:read :write}
          :deny-globs ["**/.ssh/**" "**/.env" "**/secrets/**"]}
  :net   {:allow false}
  :clock {:allow true  :mode :virtual}
  :rand  {:allow true  :mode :seeded}}}
```

## Console model

A "REPL" interaction is:
- change docs
- watch events stream update
- approve gates by editing files
- rerun/replay

#console #repl #permissions
