# Browser adapter + harness

Created with the assistance of an AI.

## Browser adapter events

- `:browser/session-started` / `:browser/session-finished`
- `:browser/console`
- `:browser/network` (request/response)
- `:browser/dom-snapshot`
- `:browser/screenshot` (optional)
- `:browser/dom-diff` (artifact-backed)

## Browser harness projection

Runs:
- dev server start/attach
- browser session
- scripted steps (navigate, wait, snapshot, click, assert)
- emits browser events into same run folder

Example steps:

```clojure
{:steps
 [{:op :navigate :url "http://localhost:5173"}
  {:op :wait-ms :ms 300}
  {:op :dom-snapshot :tag "boot"}
  {:op :click :selector "[data-testid=run]"}
  {:op :wait-ms :ms 300}
  {:op :dom-snapshot :tag "after-run"}]}
```

#browser #devtools

## DOM snapshot diffing

Normalize DOM trees (lossy but stable), then diff.

DOM diff artifact:

```clojure
{:dom-diff/id "domdiff-01H..."
 :from {:dom/id "dom-boot" :tag "boot"}
 :to   {:dom/id "dom-after" :tag "after-run"}
 :summary {:added 3 :removed 1 :changed 5}
 :ops
 [{:op :node/add :path [0 2 1] :node {...}}
  {:op :node/remove :path [0 1 0]}
  {:op :node/attr-set :path [0 2] :attr :disabled :value true}
  {:op :node/text-set :path [0 3] :text "Done"}]}
```

## Linking model (xrefs)

Maintain `.lineara/runs/<run>/xrefs.edn` mapping:
- event id -> links
- artifact hash -> path/mime
- docs -> path/blob/line ranges

#xrefs #provenance
