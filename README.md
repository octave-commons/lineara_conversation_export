# Lineara conversation export

Created with the assistance of an AI.

This archive captures the design/spec content produced in this conversation (January 26, 2026) and organizes it into Obsidian-friendly Markdown notes.

## Contents

- `docs/lineara-contract.md` — determinism boundary, observables, capability model
- `docs/a-ir-v0.md` — A-IR v0 schema, ANF lowering rules, Wasm lowering, pretty-print strategy, Spec IR v0
- `docs/events-and-replay.md` — `events.ednlog` format, replay keys, blob storage
- `docs/review-gates-and-policy.md` — review gates, capability policy, console model
- `docs/tui-mvp.md` — OpenTUI + React + shadow-cljs TUI spec (panels, commands, indexing, projections)
- `docs/browser-adapter.md` — browser adapter event contracts, harness projection, DOM snapshot diffing, linking model
- `docs/run-lifecycle-and-sandbox.md` — run lifecycle state machine, gate auto-generation rules, projection runner sandbox envelope
- `TRANSCRIPT.md` — curated transcript (user prompts + assistant outputs, lightly edited for structure)

## Notes

- Hashtags are included as plain text (not in code fences) so Obsidian graph linking works.
- Code blocks are used only where syntax is important.

## Related projects

- [promethean-agent-system](https://github.com/octave-commons/promethean-agent-system) — agent runtime and tool registry that can consume Lineara contracts.
- [ollama-benchmarks](https://github.com/riatzukiza/ollama-benchmarks) — benchmark source data that informs agent policy and evaluation.
- [cephalon-clj](https://github.com/octave-commons/cephalon-clj) — Discord IO bridge that can adopt Lineara run lifecycle + events model.
