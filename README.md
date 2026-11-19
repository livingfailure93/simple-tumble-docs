# Simple Tumble

**Rolling cube puzzles for Unity that are generated, solved exactly and rated.**

Simple Tumble builds a rolling cube puzzle from your settings, then proves it can be solved and works out its **par**, the
fewest rolls it takes. The cube tips from tile to tile, and the puzzle is won with the right face down on the goal or
pressed onto every target. It can add keys and doors, switches and gates, avoid and one-way tiles, multi-face tiles,
turntables, crumbling tiles and portals, on square boards or shaped ones. Every puzzle is rated 1 to 5 stars and can be
saved as a ScriptableObject, exported as a PNG or SVG picture, or played in the included 3D demo.

*Documentation:* `Window > Living Failure > Simple Tumble > Documentation` (offline HTML, in `Documentation/`).

---

## The rules

| Mode | To win |
|---|---|
| **ReachGoal** | roll the cube onto the goal tile with the goal face down. |
| **StampAll** | land on every target (1 to 8 of them) with the goal face down to stamp it, in any order or in their numbered order. |

Every roll tips the cube over one of its bottom edges onto the next tile north, east, south or west, so it changes which
face is down. Walls, holes and the board's edge block the cube. Boards go from 2 to 24 tiles each way, as a full
rectangle, a cross, ring, diamond, circle, heart, L, T or U, or any outline you type in. Every face has a shape as well
as its colour, in the pictures and in the demo, so colour-blind players can tell them apart.

| Gimmick | What it does |
|---|---|
| **Keys and doors** | rolling onto a key takes it and opens its doors for good (up to 4 keys). |
| **Switches and gates** | every press of a switch flips its gates open or shut (up to 4 switches, 1 to 3 gates each). |
| **Face locks** | a key or a switch only answers one face. |
| **Avoid tiles** | one face must never be down on them. |
| **One-way tiles** | crossed only in the direction of their arrow. |
| **Multi-face tiles** | need 2 or 3 faces pressed on them before the puzzle can be won. |
| **Turntables** | turn the cube a quarter turn in place when it rolls on. |
| **Crumbling tiles** | hold the cube once and break when it rolls off. |
| **Portals** | rolling onto one, the cube comes out of its twin and rolls once more the same way (up to 2 pairs). |

---

## What makes it "verified"

- **The par is real.** The solver searches every position a puzzle can reach, so a 30-roll puzzle really takes 30 rolls.
  The generator always gives it room for every position the board can hold, so its puzzles are never cut short.
- **Getting stuck is measured.** One-way tiles, switches, turntables and crumbling tiles can strand the cube. Every
  puzzle reports whether that can happen, and `NoSoftLocks`, on by default, only accepts puzzles where it can't.
- **Every gimmick matters.** Each one is placed where it makes the puzzle longer, and one that changes nothing from the
  start is taken away. Turntables, crumbling tiles and portals are placed as tools the best solution needs.
- **Stars from measurement.** The 1 to 5 star rating comes from the par and the rolls spent turning the cube beyond the
  plain walk, with bands measured over 600 boards. Ask for a star count and the generator keeps trying until it gets it.
- **Misses are reported.** When a star count, a par window or a gimmick that doesn't fit can't be met, the puzzle's notes
  say what happened and what you got instead.

---

## Quick start

1. Open **Window > Living Failure > Simple Tumble > Tumble Generator**.
2. Press **Presets**, pick one (for example *Gimmicks/Keys and doors*) and press **Generate**. Puzzles are built and solved
   on a background thread, with a progress bar, Cancel and a time limit.
3. Read the analysis pane: the par, the stars, a picture of the puzzle in 2D or 3D, the cube unfolded, and a slider that
   steps through the best solution.
4. **Save** the selected puzzle as an asset, or the whole set as a pack, and **Export** PNG or SVG pictures.
5. Open `Scenes/SimpleTumbleDemo.unity` and press Play.

### From code

```csharp
using SimpleTumble;

var settings = new TumbleSettings
{
    Width = 7, Height = 7,
    Keys = 1,                    // a key that opens a door on the way
    TargetDifficulty = 3,        // keep trying seeds until it rates 3 stars
};
TumbleData puzzle = TumbleGenerator.GenerateVerified(settings);
Debug.Log($"Par {puzzle.Par} rolls, {puzzle.Difficulty} stars, solvable: {puzzle.Solvable}");
```

Run your own game on it with the same rules the solver used, so your game always agrees with the par:

```csharp
TumblePlay play = TumbleRules.NewGame(puzzle);
if (TumbleRules.TryRoll(puzzle, play, 1, out string reason))   // 0 north, 1 east, 2 south, 3 west
{
    if (TumbleRules.IsSolved(puzzle, play)) Debug.Log("Solved!");
}
else Debug.Log(reason);                                         // why not: a wall, a hole, a shut door...
```

`TumbleGenerator` also has `Generate`, `GenerateBatch` and an `Async` version of each, which run on a background thread
and can be cancelled. `TumbleRules.Hint` gives the first roll of a best way on from any position.

---

## The demo

One scene and one component, `TumbleDemo`, which builds everything in code: no prefabs, models, textures or audio files.
It opens on a welcome screen, then four modes:

| Mode | What you play |
|---|---|
| **Classic** | Reach the goal with the right face down. |
| **Stamp** | Stamp every target with the right face, and sometimes in order. |
| **Locks** | Keys and doors, switches and gates, and face locks. |
| **Tricky Tiles** | One-way and avoid tiles, turntables, crumbling tiles, portals and multi-face tiles. |

Each mode has **Endless** puzzles made to your choice of a square or shaped board and difficulty, and a **Level Pack**.
The demo ships with 44 levels. Play has undo, restart, hints, a solver that can play the rest for you, a camera you can
turn, tilt and zoom, rolls against par, and sounds and effects made in code.

Controls: arrow keys, WASD, a swipe or a tap beside the cube to roll. U undo, R restart, H hint, F solve, [ ] turn,
Home / End tilt, wheel zoom, V reset the view, M mute, Esc for the menu.

---

## Layout

```
SimpleTumble/
├─ Runtime/
│  ├─ Api/               TumbleGenerator, TumblePresets (and Auto), TumbleAudit
│  ├─ Data/              TumbleSettings, TumbleData
│  ├─ Generation/        the builder, board shapes and trim, placing every gimmick, Rng
│  ├─ Solving/           solver and star rating
│  ├─ Rules/             Cube, TumbleRules and TumblePlay: the play rules
│  ├─ Presentation/      TumbleExport (PNG and SVG), TumbleSketch, TumbleView3D, TumbleLook (colours and shapes)
│  ├─ ScriptableObjects/ puzzle asset, pack, IPuzzleProvider
│  └─ Demo/              3D board, game, HUD, hub, sounds, effects, HDRP look
├─ Editor/               Tumble Generator window, analysis pane, inspectors, documentation menu, build look keeper
├─ Documentation/        offline HTML docs
├─ Generated/            the demo's four level packs
└─ Scenes/               the demo scene
```

Three assemblies: `SimpleTumble` (the core, with no package dependencies), `SimpleTumble.Demo` (Input System and Unity
UI, plus HDRP when it is installed) and `SimpleTumble.Editor`.

---

## Requirements

- Unity **6000.6** or newer.
- **URP** (recommended), **HDRP** and the **Built-in** pipeline. The demo uses the active pipeline's default lit shader
  and keeps it in builds, and on HDRP it adds a small volume so it looks the same.
- Demo only: the Input System package (set Active Input Handling to *Input System Package* or *Both*) and Unity UI.

## Limits, honestly

- Boards up to 24 x 24 with up to 8 targets, 4 keys, 4 switches, 4 multi-face tiles, 4 turntables, 4 crumbling tiles and
  2 portal pairs. Targets, keys, switches, multi-face faces and crumbling tiles count 10 at most together, because each
  one doubles the positions the solver weighs. **Auto** cuts a request down to fit and says what it changed.
- Gimmicks add rolls, so puzzles with them rate higher: two keys or switches rarely rate below 3 stars, and a multi-face
  tile rarely below 3.
- Measured on a desktop PC: most presets build in 2 to 46 ms, portals in about 80 ms, crumbling tiles in up to 0.3 s, and
  the Everything preset in 0.3 to 0.7 s. Many targets in any order together with keys and switches can take from several
  seconds to over a minute a board, so the window has a time limit (60 s by default) and keeps the closest board.
- A tight trim gives fewer best solutions, not one: measured on 12 x 12 boards, a single best solution in 6 of 40.

## Support

**livingfailuregames@gmail.com**. Please include your Unity version and, for anything about a particular puzzle, its seed
and settings, since a seed rebuilds the puzzle exactly.

Licensed under the Unity Asset Store EULA. © Living Failure.
