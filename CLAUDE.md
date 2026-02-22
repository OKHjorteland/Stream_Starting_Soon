# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file OBS Browser Source overlay for a "Stream Starting Soon" scene for the Oddmeister TV stream. There is no build system, no dependencies, and no package manager — the HTML file is loaded directly into OBS as a Browser Source.

## Development Workflow

Open `Stream_Starting_Soon.html` directly in a browser for previewing changes. OBS reloads the source when you refresh the browser source or toggle it.

There are no lint, build, or test commands.

## File Structure

- `Stream_Starting_Soon.html` — the entire overlay (HTML + CSS + JS, self-contained)
- `assets/` — PNG images for the smiley logo variants:
  - `Smiley.png` — default smiley
  - `Smiley_Happy.png` — used at the 1-minute mark
  - `Smiley_Cruel.png` — alternate smiley variant

## Branches

- `main` — full center-stage design: large smiley, centered countdown, progress bar, sparkle burst effect, flame overlay, smiley swap at 60 s, "Let's Go!" text at 0
- `complex-version` — corner dock UI: bottom-left frosted-glass dock, floating embers canvas, fog drift, no flame overlay asset

## Architecture

The HTML file has three sections:

1. **CSS** — uses `:root` CSS custom properties for the color palette (`--orange`, `--orange2`, `--cream`, background vars). The `last10` / `finalTen` class is toggled on the root element to trigger "final 10 seconds" visual states.

2. **HTML** — the DOM is minimal. Layers from bottom to top: animated gradient background → fog div → embers canvas → vignette → heat overlay → dock/content UI.

3. **JavaScript** — two self-contained systems:
   - **Countdown**: driven by `setInterval`. `totalStart` (line ~255) is the only value to change for a different duration (currently `5 * 60`). At 10 s remaining it adds `last10` class to root. At 0 it stops and shows end state.
   - **Embers**: a `requestAnimationFrame` canvas particle system. `MAX` controls the ember count. Each ember has randomised position, velocity, hue, size, and lifetime. Spawning is weighted toward a central hotspot (55% chance).

## Color Palette

| Var | Value | Use |
|---|---|---|
| `--orange` | `#ff7a18` | Primary accent, live dot, ember hue base |
| `--orange2` | `#ffb000` | Secondary warm accent |
| `--cream` | `#ffe9d2` | Text highlight |
| `--bg1/2/3` | navy/purple/teal | Background gradient stops |
