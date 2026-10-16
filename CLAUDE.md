# Abyssal Depths (repo: dungeon-crawler)

A turn-based roguelike dungeon crawler Tim built and plays on his iPhone and laptop. Pick a class (Warrior, Rogue, Paladin, Ranger), go down 10 random floors, kill the Abyssal Dragon on floor 10. Single player, no accounts, no backend.

- **Live:** https://timothyhadfield.github.io/dungeon-crawler/ (GitHub Pages, deploys from `main` root on push)
- **Local folder:** `C:\Users\timha\OneDrive\Desktop\my-website\Code Projects\Games\Dungeon_Crawler`
- Handoff state: `progress.md` (read START HERE first).

## File map
- `index.html`: the whole game (~2,780 lines): CSS at the top, HTML screens (title, class picker, help modal, game HUD/side panel/d-pad, death/victory, level-up popup, achievement toast), then one `<script>`:
  - constants: `MAP_W/MAP_H` 60x45, `TILE_SIZE` 28, `FOV_RADIUS` 8, `MAX_FLOOR` 10, `ENEMY_DEFS`, `ITEM_DEFS`, `CLASSES`
  - sound: Web Audio tones only (`SFX`, `startAmbient`), no audio files
  - `createMap` (rooms + L corridors, water, hidden traps, shrine, treasure vault), `computeFOV` (ray cast), `spawnEnemies`, `spawnItems(map, floor, occupied, rooms, passive)`
  - `initGame`, `descendFloor`, `tryMove`, `waitTurn`, `autoExplore` (BFS, Tab), `playerAttack`, `enemyAttack`, `checkBossPhase`, `checkLevelUp`/`showLevelUpChoices`, items (`pickupItem`, `useItem`, `dropItem`), `recalcStats` (class passives), `processEnemyTurns`, `endTurn`
  - rendering: `render` (canvas, requestAnimationFrame loop), `renderMinimap`, `updateHUD`, `updateInventoryUI`
  - input: keyboard handler, d-pad touch + click handlers (touch suppresses the follow-up click for 500 ms)
- `README.md` + `docs/screenshots/*.png`: the public showcase page on GitHub.

## Saved data
Browser `localStorage` only: `abyssal_depths_scores` (top 10 runs) and `abyssal_depths_achievements`. Nothing leaves the device.

## Run locally
`python -m http.server 8103` in this folder (start it with the background option), then open http://localhost:8103/.

## Test
No test suite and no build step (don't add a framework). Verify with phone-view:
- `node ~/.claude/tools/phone-view.mjs http://localhost:8103/ --out shot.png` (iPhone) and `--desktop`.
- Reach the game: `--do "click:.menu-btn" --do "wait:#class-picker.active" --do "click:.class-card:nth-child(1)" --do "wait:#game-container.active"`.
- Game logic can be driven with `--eval`: top-level `let` globals (`game`, `selectedClass`, `canvas`, `ctx`, `minimapCanvas`, `minimapCtx`) are reachable. Set the canvases, then call `initGame()`, `tryMove`, `waitTurn`, `descendFloor`, etc. Seed `Math.random` for repeatable maps.

## Traps
- On phones (width 768px or less) the side panel (stats, inventory, minimap) is hidden, so there is no way to use potions or see the inventory on a phone. Known gap, Tim's call (see progress.md).
- The class picker is a fixed full-screen flex column; on a phone the four cards are taller than the screen, so it must stay scrollable (`justify-content: safe center; overflow-y: auto`).
- `body` is `100vh` with `overflow: hidden`; the canvas is sized from its container in `resizeCanvas()`.
- `addMessage` keeps only the last 6 lines in `#message-log`; `initGame` clears it so a new run doesn't show the old run's lines.
- `recalcStats()` must run after enemy turns, or the Warrior's Berserker bonus lags a turn behind HP.
- The title screen's particle animation never idles, so a Lighthouse run takes 5+ minutes; run it in the background. Live scores 2026-10-16 (mobile): performance 76, accessibility 85, best practices 96. The perf cost is the title animation (Tim's visuals, left alone); the a11y hit is `user-scalable=no` in the viewport meta (left for Tim).
