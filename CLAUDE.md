# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of retro-style browser games built with vanilla HTML5 Canvas and JavaScript — no build tools, no dependencies, no frameworks. Each game is a single self-contained `.html` file.

## Running Games

Open any `.html` file directly in a browser:

```bash
open shooter.html
open tictactoe.html
```

No server, no build step, no install required.

## Git & GitHub Workflow

All changes must be committed with clean messages and pushed to `https://github.com/sevak-epam/browser-games` after every meaningful change.

```bash
git add <file>
git commit -m "descriptive message"
git push
```

Commit granularity: one logical change per commit (e.g. "add enemy health bars", not "various fixes").

## Architecture

Each game is fully self-contained in a single HTML file with inline `<style>` and `<script>`. The pattern used in `shooter.html`:

- **Constants & config** — canvas size, game state enum, per-entity config objects, level generator function
- **Input module** — `keys` object (keydown/keyup), `mouse` object (mousemove/mousedown/mouseup), canvas-relative coordinate scaling
- **Entity classes** — `Player`, `Enemy`, `Bullet`, `Particle`; each has `update(dt)` and `draw()` methods
- **Game functions** — `startGame()`, `startLevel()`, collision detection, explosion spawner, enemy queue builder
- **Render functions** — one function per screen (`drawMenu`, `drawUI`, `drawLevelComplete`, `drawGameOver`, `drawBackground`)
- **Game loop** — `requestAnimationFrame` loop with delta-time (`dt`) capped at 50 ms to prevent spiral-of-death on tab resume

All rendering uses `ctx.shadowBlur` + `ctx.shadowColor` for neon glow; `ctx.globalAlpha` for trails/particles. No sprite sheets — everything is drawn procedurally with Canvas 2D primitives.

## Adding a New Game

Create a new `.html` file following the same single-file pattern. Reuse the same game loop structure (state machine + delta-time RAF loop).
