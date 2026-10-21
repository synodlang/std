# std

The Synod standard library, the module `github.com/synodlang/std`. Programs import it as `std/<package>`.

| Package | What it holds |
| --- | --- |
| `sim` | nodes, runs, faults; the core trace events |
| `net` | the simulated network; `Received` and the network trace events |
| `time` | `Now`, `Every` |
| `random` | deterministic randomness |
| `lists`, `maps`, `math`, `text` | list, map, number and text phrases |
| `disk` | storage that survives crashes |
| `lesson` | Predict mode and captions |
| `theme` | named colors |
| `look` | shapes, motion, layout (look context only) |
| `look/basic` | the default look |

The catalog is normative: `std/README.md` in [dsl](https://github.com/synodlang/dsl). Every pattern in it has exactly one `define` here. Phrases marked N in the catalog are `native` definitions whose ids the compiler implements; phrases marked S are written in Synod.

The sources are vendored into the compiler (`internal/stdlib/std`) and embedded in the `synod` binary; a drift test keeps the copy equal to this repository.

Implements the Synod language specification v0.2.0: https://github.com/synodlang/dsl

Licensed under the [Apache License 2.0](LICENSE).
