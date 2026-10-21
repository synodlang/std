# std

The Synod standard library: nodes, networks, time and the everyday words of a program.

[![ci](https://github.com/synodlang/std/actions/workflows/ci.yml/badge.svg)](https://github.com/synodlang/std/actions/workflows/ci.yml)
[![licence](https://img.shields.io/badge/licence-Apache--2.0-blue)](LICENSE)

This is the module `github.com/synodlang/std`. Programs import its packages
as `std/<package>`. Nothing is installed: the compiler carries a copy of
these sources inside the `synod` binary.

## Where it fits

The standard library is ordinary Synod packages plus a few native phrases.
The language itself is a fixed set of words; everything you write with, such
as sending a message or waiting a second, is a phrase defined here.

| Repository | Its part |
| --- | --- |
| [dsl](https://github.com/synodlang/dsl) | The catalog of std phrases (`std/README.md`), normative |
| `std` (this one) | The sources: one `define` for every pattern in the catalog |
| [compiler](https://github.com/synodlang/compiler) | Embeds a copy of these sources and implements the native phrases |
| [examples](https://github.com/synodlang/examples) | Programs that import `std` |

## Packages

| Package | What it gives |
| --- | --- |
| `sim` | Nodes, runs and faults: `Spawn`, `Cluster of`, `Crash`, `Restart`, `Remove`, `Seed`, `StopAt`, `Nodes`, `IsUp`; the core trace events (`Started`, `Added`, `Changed`, `Broken`, `Failed`, ...) |
| `net` | The simulated network: `Send`, `Latency`, `Loss`, `Partition`, `Heal`, `DropNext`; the `Received` event and the network trace events |
| `time` | `Now` and `Every` |
| `random` | Deterministic randomness: `Pick`, `Between`, `TimeBetween`, `Chance` |
| `lists` | `Size`, `Append`, `Extend`, `Without`, `Take`, `Range`, `Sort` |
| `maps` | `Size`, `Keys`, `Without`, `KeysByValue` |
| `math` | `Floor`, `Ceil`, `Min of`, `Max of`, `Remainder of`, `Hash` |
| `text` | `From`, `Join`, `Size` |
| `disk` | Storage that survives crashes: `Write`, `Read`, and the `Written` trace event |
| `lesson` | Predict mode and captions: `Ask` and `Note` |
| `theme` | Named colors: `Ink`, `Paper`, `Line`, `Quiet`, `Accent`, `Good`, `Warning`, `Critical` |
| `look` | Shapes, motion and layout, for look context only |
| `look/basic` | The default look |

Each package is a folder with one or more `.synod` files
(`sim/sim.synod`, `time/time.synod` and `time/every.synod`, and so on).
The tables with every pattern, its result type, the contexts where it may be
used and what it means are in the
[catalog](https://github.com/synodlang/dsl/blob/main/std/README.md).

## Natives and Synod

A phrase is either written in Synod or declared `native`.

- A **native** has no body here, only its pattern and an id:
  `define Send [m: msg] from [a: node] to [b: node] native "net.send"`. The
  compiler implements the id. Every phrase of `sim`, `net`, `random`,
  `lists`, `maps`, `math`, `text` and `disk` is native, and so are `time.Now`
  and the shape phrases of `look`.
- A phrase **written in Synod** is a normal definition. `time.Every`,
  `lesson.Ask`, `lesson.Note`, `theme` and the motion helpers of `look`
  (`Fade`, `Pulse`, `Travel`, `Remove`, `Card for`) are examples, so you can
  read exactly what they do:

```synod
# package time
define Every [d: time] [b: block]
  b
  after d
    Every d
      b
```

In the catalog, N marks the first kind and S the second. Only std packages
may use `native` and the type `value`; elsewhere both are errors.

The `look` phrases check today, but the simulator does not run them yet: they
arrive with the scene builder (milestone M5 of the
[roadmap](https://github.com/synodlang/dsl/blob/main/implementation/10-roadmap.md)).

## A short example

A package that sends a heartbeat to a random peer once a second, and a
program that runs three of them on a lossy network. It uses `net`, `random`,
`time` and `sim`.

```synod
# package ping
use std/net
use std/random
use std/time

Beat has
  set Count to 0

Node has
  set Seen to 0
  set peers to {}

define Join [n: Node] to [other: node]
  set n.peers to {other}

define [n: Node] start
  time.Every 1s
    net.Send (message Beat with Count n.Seen) from n to (random.Pick n.peers)

when [n: Node] (net.Received Beat from a)
  set n.Seen to (n.Seen plus 1)
```

```synod
# package main
use example.com/demo/ping
use std/net
use std/sim

define main
  net.Latency between 10ms and 40ms
  net.Loss 0.2
  set nodes to (sim.Cluster of 3 running ping)
  ping.Join (item 0 of nodes) to (item 1 of nodes)
  ping.Join (item 1 of nodes) to (item 2 of nodes)
  ping.Join (item 2 of nodes) to (item 0 of nodes)
  sim.StopAt 5s
```

Put them in a module named `example.com/demo` (a `synod.mod` with
`module example.com/demo` and `synod 0.2`), as `ping/ping.synod` and
`ping/demo.synod`, and run it:

```
synod run ping --summary
```

## Staying in step

There are three copies of the same text, and tests keep them equal:

- The **catalog** in `dsl` (`std/README.md`) says what each phrase is.
- These **sources** have exactly one `define` for every pattern in it.
- The compiler **embeds** a copy of these sources in
  `internal/stdlib/std`.

In the compiler repository, `TestCatalog` checks the sources against the
catalog, and `TestStdDrift` fails when the embedded copy differs from this
repository. They run in CI with `SYNOD_DSL` and `SYNOD_STD` pointing at the
other checkouts.

So a change to a phrase goes in this order: the catalog in `dsl`, then these
sources, then the compiler's embedded copy (and its native, if the phrase is
native). Keep files in canonical form (`synod fmt --check` passes in this
repository), with LF line endings, no trailing spaces and a final newline.
CI checks the text rules.

## Why Synod

Synod is named after the Synod protocol, the core consensus algorithm in Leslie Lamport's paper "The Part-Time Parliament" (ACM Transactions on Computer Systems, 1998; first submitted in 1990). The paper tells it as the story of the parliament of the ancient Greek island of Paxos, whose part-time legislators could agree on decrees although they came and went; today the algorithm is known as Paxos. Lamport later explained it plainly in "Paxos Made Simple" (2001), and in 2013 he received the ACM A.M. Turing Award for fundamental contributions to the theory and practice of distributed and concurrent systems. His "Time, Clocks, and the Ordering of Events in a Distributed System" (1978) and his TLA+ specification language shaped how we reason about such systems. We are grateful for his work, and we hope Synod honours it by making these ideas easier to watch and understand. The standard library gives the language its everyday words for nodes, networks and time, the vocabulary his ideas are described in.

Implements the Synod language specification v0.2: https://github.com/synodlang/dsl

Licensed under the [Apache License 2.0](LICENSE).
