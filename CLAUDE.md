# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vanilla-JS Tetris. No dependencies, no build step, no package.json. Three files: `index.html` (DOM/canvas), `style.css` (dark/retro theme), `game.js` (all game logic, ~300 lines).

## Running

Open `index.html` directly in a browser, or serve statically:

```bash
python3 -m http.server 8000
npx serve .
php -S localhost:8000
```

There is no test suite, linter, or build command in this repo.

## Architecture (`game.js`)

Single file, no modules, everything in global scope, `'use strict'`.

- **Board model**: `ROWS × COLS` matrix; each cell is `0` (empty) or a color index `1–7` identifying the locked piece type.
- **Pieces**: `PIECES` holds the 7 standard tetrominoes as square matrices; `COLORS` maps piece index → hex color. Rotation (`rotateCW`) is a transpose + row-reverse, not per-piece rotation tables.
- **Collision** (`collide`): checks board bounds and overlap against already-locked cells.
- **Wall kicks** (`tryRotate`): after rotating, tries offsets `[0, -1, 1, -2, 2]` columns until one doesn't collide, else the rotation is discarded.
- **Game loop** (`loop`): driven by `requestAnimationFrame`, accumulates `dt` and advances the piece one row once `dropAccum >= dropInterval`.
- **Locking** (`lockPiece` → `merge` + `clearLines` + `spawn`): merges the current piece into the board, clears completed rows (shifting from the bottom up, re-checking the same row index after a splice), then spawns the next piece.
- **Scoring/leveling**: `LINE_SCORES[cleared] * level` on line clear; level increments every 10 lines; `dropInterval = max(100, 1000 - (level-1)*90)`. Hard drop adds `2 * rows_dropped`; soft drop adds `1` per row.
- **Ghost piece** (`ghostY`): projects the current piece straight down to its landing row; drawn at `globalAlpha = 0.2`.
- **Game over**: detected in `spawn()` when the newly spawned piece immediately collides at its spawn position.

All module-level mutable state (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, timing vars) lives in one set of `let` declarations near the top and is reset in `init()`.

Tunable constants at the top of `game.js`: `COLS`, `ROWS`, `BLOCK` (px per cell), `COLORS`, `LINE_SCORES`, initial `dropInterval`. If `COLS`/`ROWS`/`BLOCK` change, update the `width`/`height` attributes of `<canvas id="board">` in `index.html` to match (`COLS × BLOCK` by `ROWS × BLOCK`).
