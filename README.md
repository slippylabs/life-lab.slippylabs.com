# Cellular Automata Lab

Conway's Game of Life and any other B/S rule you can name — draw cells, drop in gliders and guns, watch the population, and export what you built as RLE. Runs entirely in your browser.

**Live:** <https://life-lab.slippylabs.com/>

## What it does

- Any two-state, 8-neighbour rule: `B3/S23` (Life), HighLife, Day & Night, Seeds, Replicator, Diamoeba, Maze, Life without Death, and whatever else you type.
- A pattern library — glider, LWSS, pulsar, penta-decathlon, r-pentomino, acorn, diehard, Gosper glider gun.
- Draw with the mouse or a finger while it runs; hold Shift to erase.
- Torus or dead-border edges, grids up to 256 × 192, speed from 1 generation per second to flat out.
- Population history, and automatic detection of still lifes and oscillators by hashing each state.
- RLE import and export, so anything from a pattern catalogue can be pasted straight in.

## How it works

The grid is one `Uint8Array` drawn a device pixel per cell into an `ImageData` and scaled up by CSS with `image-rendering: pixelated` — much cheaper than a `fillRect` per cell, and the cells stay square at every zoom. Cycle detection uses a 53-bit hash built from two independent FNV-1a passes; 32 bits alone would eventually collide and invent an oscillator that was not there.

## Verification

The stepper was checked cell for cell against a numpy reference (eight shifted whole-array adds, rather than a per-cell loop) across every shipped rule, both edge modes and 40 generations of random soup, plus 200 generations of every shipped pattern. The reference is itself anchored to published facts: diehard dies at generation 130, the r-pentomino settles at 1103 with 116 cells, acorn at 5206 with 633, and the pulsar and penta-decathlon have periods 3 and 15. Every shipped pattern is also compared against the catalogue RLE for the same pattern, so a mistyped row cannot hide.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/life-lab.slippylabs.com.git
cd life-lab.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
