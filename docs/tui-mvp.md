# TUI MVP spec: Lineara Devtools

Created with the assistance of an AI.

## Shape

- Host app: TS/JS (bun/node)
- Renderer: OpenTUI + React reconciler
- UI logic: CLJS via shadow-cljs, exporting a React root module

## Layout

- Left: Navigator (Runs / Projections / Files / Browser targets)
- Center: Tabs (Trace / Artifacts / Browser / Diff / Gates)
- Right: Inspector (selected item details)
- Bottom: Command palette + status

#tui #devtools #opentui

## Keybindings (v0)

- `j/k` move
- `h/l` history back/forward
- `tab` next pane
- `/` search
- `:` command palette
- `a` approve gate
- `x` reject gate
- `r` run current projection

## Panels

### Runs
- lists `.lineara/runs/*`
- derives status from events
- opens Trace by default

### Trace
- tail `events.ednlog`
- timeline + tree view
- filters by kind/cap/agent/text
- inspector resolves links: artifacts, gates, related events

### Gates
- scans `reviews/` for `status: pending`
- approve/reject edits gate file and emits `:gate/updated`

### Artifacts
- universal artifact viewer:
  - markdown
  - EDN/JSON tree
  - code/text
  - diff/patch
  - binary (metadata + open external)

### Projections
- runtime adapters are dumb:
  - detect
  - run
  - export
- everything emits events and writes artifacts into run folder

## Command palette

Commands are `domain.verb`, e.g.
- `run.open`
- `gate.approve`
- `projection.run`
- `artifact.open`
- `browser.session.start`

## Event indexing

Maintain an in-memory index per run:
- byte offsets per event line
- `by-id` map
- optional lazy `children` map for tree reconstruction

#commands #indexing #projections
