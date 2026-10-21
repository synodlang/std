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

## Why Synod

Synod is named after the Synod protocol, the core consensus algorithm in Leslie Lamport's paper "The Part-Time Parliament" (ACM Transactions on Computer Systems, 1998; first submitted in 1990). The paper tells it as the story of the parliament of the ancient Greek island of Paxos, whose part-time legislators could agree on decrees although they came and went; today the algorithm is known as Paxos. Lamport later explained it plainly in "Paxos Made Simple" (2001), and in 2013 he received the ACM A.M. Turing Award for fundamental contributions to the theory and practice of distributed and concurrent systems. His "Time, Clocks, and the Ordering of Events in a Distributed System" (1978) and his TLA+ specification language shaped how we reason about such systems. We are grateful for his work, and we hope Synod honours it by making these ideas easier to watch and understand. The standard library gives the language its everyday words for nodes, networks and time, the vocabulary his ideas are described in.

Implements the Synod language specification v0.2.1: https://github.com/synodlang/dsl

Licensed under the [Apache License 2.0](LICENSE).
