# Changelog: Simple Tumble

All notable changes to this package.

---

## [1.0.0]

First release.

### Puzzles

- **Two modes:** ReachGoal (roll onto the goal with the goal face down) and StampAll (stamp 1 to 8 targets with the goal
  face, in any order or in their numbered order).
- **Boards:** 2 to 24 tiles each way, with walls and holes on up to 45% of the board, as a full rectangle or cut to a
  cross, ring, diamond, circle, heart, L, T or U, or to any outline you type in. Turning room sets how many places the
  cube can turn round on the spot.
- **Gimmicks:** up to 4 keys and doors, up to 4 switches with 1 to 3 gates each, face locks, avoid tiles, one-way tiles,
  up to 4 multi-face tiles needing 2 or 3 faces, up to 4 turntables, up to 4 crumbling tiles and up to 2 portal pairs.
- **Every gimmick matters:** each one is placed where it makes the puzzle longer, the start is picked so that every one
  of them changes the puzzle, and one that still does nothing is taken away. Turntables, crumbling tiles and portals are
  placed as tools the best solution has to use.
- **Trim:** cut the board down to the puzzle, keeping every best solution (Loose, nothing about the puzzle changes) or
  one (Tight, fewer best solutions and the stars re-rated).
- Every face has a colour and a shape, and every gimmick its own shape, so colour-blind players can tell them apart.

### Solving and rating

- An exact solver over every reachable position, every gimmick included. It gives the **proven par**, one best solution,
  the number of best solutions with a completeness flag, and whether a player **can get stuck**.
- The generator, hints and the inspectors' Verify always give the solver room for every position the board can hold, so
  their results are never cut short.
- **Difficulty** (1 to 5 stars) from the par and the rolls spent turning the cube beyond the plain walk, with bands
  measured over 600 boards, and separate bands for each number of targets in StampAll.

### Generation

- `TumbleGenerator.Generate`, `GenerateVerified` (keeps trying seeds until a puzzle is solvable, inside the par window,
  free of soft locks when asked, has every gimmick it asked for, and is on its star target) and `GenerateBatch`, each
  with a background-thread `Async` version that can be cancelled. `GenerateVerified` can also report what each board is
  doing and stop at a time limit.
- Deterministic: the same settings and seed always give the same puzzle, and every puzzle stores the seed that made it.
- Every miss is written to the puzzle's notes. Nothing is quietly downgraded.
- 20 presets, each built 5 times in testing with every build getting everything its preset asked for, and **Auto**,
  which fixes settings that fight each other and says what it changed. Every fix comes from a measurement.

### Playing

- `TumbleRules`: the play rules your game uses, the same ones the solver counted with. It covers rolls, why a roll isn't
  allowed, keys, switches, stamps, presses and crumbled tiles, hints from any position (`Hint`), the win check and a data
  validator.
- `Cube`: the rolling law, so your game can read which face is where after any roll or turn.

### Editor

- The **Tumble Generator** window: every setting with tooltips, a live warning of what Auto would change, presets, a seed
  you can keep, batches of up to 50 built on a background thread with progress, Cancel and a time limit, Only on target,
  and Save as a puzzle or a pack.
- The **analysis pane**, in the window and in both inspectors: the verdict, a picture of the puzzle in 2D or with a real
  3D cube, the cube unfolded with a plain-words legend, a slider that steps through the best solution, hover to read a
  tile, zoom, the roll list with Copy solution, Copy seed, and the notes.
- **Export** as PNG or SVG: the colour view, a printable black on white sheet that draws every face and gimmick by its
  shape alone, or an answer key with the best solution drawn on. A pack exports every puzzle with its answer key in one
  go.
- **Inspectors** with Verify (solve again and restamp the verdict) and Re-roll (a new puzzle from the same settings),
  including re-rolling every puzzle in a pack that missed its request.

### Demo

- One scene and one component, `TumbleDemo`, which builds everything in code with no prefabs, models, textures or audio
  files.
- A welcome screen, then four modes: Classic, Stamp, Locks and Tricky Tiles. Each has Endless puzzles made to your choice
  of a square or shaped board and difficulty, and a level pack. The demo ships with 44 levels.
- The cube tips, keys fly, doors and gates sink, tiles crumble, and the cube goes through portals and turns on
  turntables. Play has undo that rolls back, restart, hints, a solver that can play the rest, a camera you can turn, tilt
  and zoom, and rolls against par with a rating.
- Keyboard, swipe and tap controls that follow the camera, so they stay right however it is turned.
- Sounds and effects made in code, the same in every render pipeline.
- Works in URP, HDRP and the Built-in pipeline. On HDRP a small volume gives it the same look, and a build step keeps the
  demo's shader in builds so it never draws pink.

### Documentation

- Offline HTML documentation (Home, Quick Start, Generating, API Reference, Demos, Troubleshooting and About, plus a
  single-page version), opened from **Window > Living Failure > Simple Tumble > Documentation**.
