# Interactive Spider Lily — Photo Bloom

A fork of [cupidbity/spiderlily](https://github.com/cupidbity/spiderlily) (web version).
MediaPipe hand tracking grows and blooms a plant, rendered full-screen with your
webcam as a small window in the corner. The only change from the original: the
procedural, vertex-colored petals/stamens have been replaced with real flower
artwork — four glowing crimson lily images (`assets/flower-1..4.png`) rendered as
camera-facing sprites that scale, twist, and fade in through the same staged
bloom choreography (tight bud → petals lift → arch → full bloom → settle).

The plant skeleton (branch growth), hand tracking, camera controls, glow
post-processing, HUD, and debug keys are otherwise unchanged from the original.

## Run it

```bash
cd spiderlily-web
python3 -m http.server 8642
# open http://localhost:8642 in Chrome/Safari and allow the camera
```

(Any static server works. Camera + hand tracking require http://localhost or
https — opening index.html directly as a file:// path will not work.)

## Interaction

- **Left hand** — pinch out (spread thumb + index) = **grow** the plant
- **Right hand** — pinch out = **bloom** the flowers
- Hands are identified by MediaPipe handedness. If your setup reports them
  backwards, set `PARAMS.interaction.swapHands = true` in `index.html`.
- The camera window shows a live tracking overlay (thumb/index dots + pinch
  line) with the grow/bloom values labeled beside each hand.
- All 3 flower heads (9 photo-flowers total) bloom **in sync** by default.
  Raise `PARAMS.flowers.bloomStagger` toward `1` to make the heads take turns;
  `PARAMS.flowers.floretBloomStagger` controls the smaller offset between the
  3 flowers within a single head (already slightly staggered for a more
  organic feel).

## Keys

| Key | Action |
|-----|--------|
| `D` | toggle debug mode (auto bloom cycle) vs live hands |
| `1` / `2` / `3` | pin bloom at bud / arching / full (debug) |
| `0` | resume the auto cycle |
| `H` | hide/show the HUD |
| drag / scroll / right-drag | orbit / zoom / pan the camera (it also slowly auto-rotates) |

## Tweaking

Everything lives in the `PARAMS` object at the top of `index.html`. New/changed
blocks vs. the original:

- `PARAMS.photo.images` — the 4 source images and their aspect ratios. Add or
  swap files in `assets/` and list them here (any count works; florets are
  assigned round-robin + shuffled).
- `PARAMS.photo.baseHeight` — world-unit size of a fully bloomed flower.
- `PARAMS.photo.budScale` / `budOpacity` — how small/faint a closed bud is.
- `PARAMS.photo.swayAmpDeg` / `swaySpeed` — gentle idle sway once open.
- `PARAMS.bloomPoses` — still drives the staged timing (bud/lift/arch/full/settle);
  its numbers are now reinterpreted as an abstract "openness" curve for the
  photo sprites instead of literal petal angles.

`PARAMS` is also exposed as `window.PARAMS` for live tweaking from the
browser console.

## Assets

`assets/flower-1.png` .. `flower-4.png` are cleaned, cropped cutouts of the
4 supplied flower images (backgrounds removed, transparent PNG). Swap in your
own art at the same aspect ratios (or update `PARAMS.photo.images[].aspect`)
to restyle the bloom.
