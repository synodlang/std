# Changelog

All notable changes to the Synod standard library. Versions share MAJOR.MINOR with the language.

## Unreleased

## v0.2.2 — 2026-10-22

Implements Synod 0.2; works with compiler v0.2.6.

### Changed

- Doc comments on `time.Every`, `lesson.Ask`, `lesson.Ask … choosing`, `lesson.Note`, `lesson.choices`, `look.Fade` and `look.Pulse`, equal to the catalog Meaning, byte for byte.
- The `disk.Read` doc comment says "the result has the type of `d`".

## v0.2.1 — 2026-10-22

Implements Synod 0.2; works with compiler v0.2.5.

### Added

- `lesson.Ask [q] choosing [c]`: asks a question with 2 to 6 distinct, non-empty choices that Predict mode offers. Anything else fails with `bad-argument`.
- `Choices` field on the `lesson.Asked` record (empty for a plain `lesson.Ask`).
- The unexported native `lesson.choices`, which checks the choices.

## v0.2.0 — 2026-10-22

Implements Synod 0.2.

### Added

- The std packages from the catalog: disk, lesson, lists, maps, math, net, random, sim, text, theme, time and look.
- `std/look/basic`, the default look, rewritten on the look natives (`look.Flight`, `net.Id`, `net.Type`, `sim.StateRows`, `look.delete`).
- Full documentation of `sim.Seed` and `math.Hash`, including the mixing finalizer.
- A README that explains the name and honours Leslie Lamport.
- `vendor/` is git-ignored.

### Changed

- Rewritten for specification v0.2.1.
