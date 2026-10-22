# Changelog

All notable changes to the Synod standard library. Versions share MAJOR.MINOR with the language.

## Unreleased

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
