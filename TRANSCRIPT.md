# Curated transcript

Created with the assistance of an AI.

This transcript is curated from the conversation and organized as a spec narrative (not a raw chat log). It captures the core decisions, structures, and contracts discussed.

## User: Naming + intent

- Proposed name: Linear A / Linear-a / Lineara
- Joke/meaning: it's “lost to time”, unreadable, translation-layer vibe
- Vision: universal translation layer, transpile large corpora, build wasm runtime, discover reversible patterns, map to languages with :oop aspects, guarantee behavioral equivalence by observables

## Assistant: Naming convention

- Human name: Linear A
- Token/cli/repo: lineara
- Optional adjective: linearan
- Extensions: .la or .lineara

## Assistant: 3-layer architecture

1. Front-end authoring: Clojure + macros
2. Canonical truth: IR
3. Backends: Wasm runtime + target printers

## Assistant: Determinism + observables

- Determinism boundary: ir hash, runtime hash, input hash, seed, policy hash
- Observables: return, log, effect requests
- Effects only via capabilities

## Assistant: A-IR v0

- EDN module
- ANF step lists
- expression set: const/var/prim/call/if/effect/loop

## Assistant: Wasm lowering

- value-tagged runtime (i64 carrier)
- locals per temp
- effects as imports
- reversibility via custom sections mapping offsets to IR node ids

## Assistant: Pretty printing

- keep intent tags
- recover structure from ANF
- deterministic naming pass
- print from A-IR; decompile from Wasm only as cross-check

## Assistant: Events + replay

- append-only events.ednlog
- cap/call + cap/return pairs
- replay keys derived from (cap, call, args, policy, world)
- blob store for large payloads

## Assistant: Review gates + policy

- gates are files in reviews/
- policy is an artifact; net off by default
- approval edits a file; pipeline resumes based on that

## Assistant: TUI MVP

- OpenTUI + React host in TS/JS
- shadow-cljs CLJS UI module exports React root
- panels: Runs, Trace, Gates, Artifacts, Browser
- command palette: domain.verb
- event indexing by byte offsets and paging

## Assistant: Browser tooling

- browser adapter emits: console/network/dom-snapshot/screenshot
- harness projection: start server, run browser session, scripted steps
- DOM snapshot diffing artifact and event
- xrefs linking model for jump-to-source behavior

## Assistant: Run lifecycle + sandboxing

- run status derived from events
- gate auto-generation triggers
- projection runner execution envelope: cwd/env allowlist, capture, limits

#transcript #spec
