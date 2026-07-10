# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Bottle.io — an agar.io-style browser game where every character is a rendered **water bottle** instead of a circle. It is a single, fully self-contained `index.html` (inline `<style>` and inline `<script>`) with **no dependencies, no build step, and no tests**.

## Running / developing

- **Play it:** double-click `index.html`. It runs directly from `file://` — the code uses only classic scripts and canvas (no ES modules, no `fetch`, no external assets), so there is nothing to serve.
- **Do not split the CSS/JS back into separate files.** They were deliberately inlined: this folder lives under OneDrive, where sibling `style.css`/`game.js` files can become cloud-only placeholders and fail to load, leaving an unstyled page. Keep everything in the one file.
- This machine has **no Node and no real Python** (the `python` on PATH is the Windows Store stub). If you need a local HTTP server for browser tooling, use the PowerShell static server in `.claude/` (`launch.json` + `serve.ps1`, serves the folder on port 8123 via `System.Net.HttpListener`). That server exists only for preview/verification tooling — the game itself never needs it, and `.claude/` can be deleted without affecting gameplay.

## Architecture (all inside one IIFE in `index.html`)

The simulation runs in world space (a `WORLD`×`WORLD` square) and is drawn in screen space via a camera. Four core entity arrays:

- **`owners`** — one per participant: the human `player` plus AI bots. Holds name, color, and (bots only) an `ai` steering-target object. Owners have no position; their position is the mass-weighted centroid of their cells.
- **`cells`** — the actual bodies (the "pieces"). An owner can have multiple cells after splitting. Each cell carries `mass`, derived `r` (`massToRadius`), split-momentum `vx/vy`, and a `mergeTimer`. **This is the key indirection:** most logic iterates `cells` and groups by `.owner`; helpers `ownerCells()` / `ownerMass()` bridge the two.
- **`food`** — static water-droplet pellets that grow whoever eats them.
- **`viruses`** — spiky hazards that split (`popCell`) any cell above `VIRUS_POP_MASS`; smaller cells pass safely. Fed by ejected mass, they bud new viruses.
- **`ejected`** — mass pellets shot with `W`; edible by cells and viruses after a short arming delay (`life`).

**Main loop:** `frame(now)` is a single `requestAnimationFrame` loop that runs even on the menu (idle background). When `running`, each frame does, in order: steer player → `updateBot` for each bot → `integrate` (momentum, world clamp, sibling merge/repel) → `handleEating` (food, cells-vs-cells with `EAT_RATIO`, viruses) → `reapOwners` (mark dead bots, refill to `BOT_TARGET`, end game on player death) → `updateCamera` → replenish food/viruses → `updateHUD`. Then it renders. Note `requestAnimationFrame` is paused by the browser when the tab/preview pane is unfocused, so the HUD "freezes" at 0 in a hidden pane — not a bug.

**Rendering:** entities are collected into one `drawList`, sorted small-mass-first (so big bottles draw on top), then drawn. `drawBottle` is the visual centerpiece — it composes a bottle from primitives (`roundRectPath` helper, no `ctx.roundRect` dependency): tinted translucent body, animated wavy water line, label band with the owner's name, tapered neck, ridged cap (white-outlined for the human). Bottles lean by horizontal velocity and bob via a per-cell `wobble` phase.

**Tuning:** gameplay constants are grouped at the top of the script (`WORLD`, `FOOD_TARGET`, `BOT_TARGET`, `START_MASS`, `MIN_SPLIT_MASS`, `MERGE_COOLDOWN`, `EAT_RATIO`, virus masses, etc.). Adjust balance there. `COLORS` and `BOT_NAMES` (water-brand-flavored) drive appearance and bot identities.
