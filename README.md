# 🍶 Bottle.io

An [agar.io](https://agar.io)-style browser game where every character is a **water bottle** instead of a blob. Sip the droplets, swallow your rivals, and climb the leaderboard.

### ▶️ [**Play it now → bottle-io.vercel.app**](https://bottle-io.vercel.app/)

No install, no sign-up — it runs instantly in your browser.

---

## Controls

| Action | Input |
| --- | --- |
| Move | **Mouse** (your bottles chase the cursor) |
| Split | **Space** |
| Eject mass | **W** |

On mobile: drag to move, tap the on-screen **SPLIT** button.

## How to play

- Roam the water world eating **droplets** to grow — the bigger your bottle, the higher your **Volume (mL)**.
- You can swallow any bottle you're at least ~20% bigger than. Stay away from anything bigger than you.
- **Split** (Space) to launch half your mass forward — great for catching prey, risky when threats are near.
- Watch out for the spiky green **☠ hazards**: touch one while you're large and your bottle bursts into pieces. Small bottles slip past them safely.
- Outlast 20 AI rivals and reach **#1** on the leaderboard.

## Features

- Every player and bot is a hand-drawn water bottle — tinted body, animated wavy water line, named label, ridged cap.
- Splitting, mass ejection, virus hazards, mass-weighted camera zoom, minimap, and a live leaderboard.
- 20 water-brand-flavored AI bots that hunt food, chase smaller prey, and flee bigger threats.

## Run it locally

It's a **single self-contained `index.html`** — no build step, no dependencies, no server needed.

```
Just double-click index.html (or open it in any modern browser).
```

## Tech

Plain HTML5 Canvas + vanilla JavaScript in one file (inline `<style>` and `<script>`). The whole simulation — physics, AI, rendering — lives in a single `requestAnimationFrame` loop. See [`CLAUDE.md`](CLAUDE.md) for an architecture overview.

---

*Built as a single-file canvas experiment. Deployed on [Vercel](https://vercel.com), which auto-redeploys on every push.*
