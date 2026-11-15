# DONE — SPEC 01: Project Setup and Menu Flow (Astro Kid)

## What Was Built

Implemented the complete Vite + Phaser 3.80.1 project and menu flow in `game/` according to `specs/SPEC-01-project-and-menus.md`:

- **Project & Build Config**
  - `game/package.json`: Plain ES module setup with `phaser@3.80.1` and `vite`, providing `npm run dev` (port `5173`) and `npm run build`.
  - `game/vite.config.js`: Configured `base: './'`, `server.port: 5173`, and `build.outDir: 'dist'` for GitHub Pages compatibility.
  - `game/index.html`: `<title>Astro Kid</title>`, `#game` container, `#060920` space background, and Google Fonts (`Press Start 2P` and `Silkscreen`).
- **Core Modules**
  - `game/src/main.js`: Waits for Google Fonts (`Press Start 2P` and `Silkscreen`, with a 3-second fallback timeout), loads persisted settings/volumes, and boots Phaser (`1280x720`, `Phaser.Scale.FIT`, `CENTER_BOTH`, `pixelArt: true`, `roundPixels: true`).
  - `game/src/config.js`: Design palette constants, font names, canvas dimensions, mission list, and storage key (`astrokid.v1`).
  - `game/src/save.js`: `SaveManager` (`load()`, `get()`, `setSetting()`, `completeMission()`, `reset()`) backed by `localStorage` with `try/catch` protection for private browsing.
  - `game/src/audio.js`: WebAudio `AudioManager` (`click()`, `denied()`, `setVolumes(music, sfx)`) with lazy `AudioContext` creation/resumption on user input.
  - `game/src/ui.js`: `pixelButton()`, `pixelPanel()`, split `ASTRO KID` title builder, and scene-scoped keyboard focus navigation (`Tab`, `Enter`, `Space`, arrow keys).
- **Scenes**
  - `game/src/scenes/BootScene.js`: Preloads `moon_background` (set to `LINEAR` filter) and `astro_walk`, then launches `LoadingScene`.
  - `game/src/scenes/LoadingScene.js`: Loads all remaining spritesheets (`astro_idle`, `mentor_idle`) and mission badges (`mission1`..`mission6` with `LINEAR` filter), registers global animations (`astro_idle`, `astro_walk`, `mentor_idle`), renders the walking Astro progress bar with smooth `0.15` per-frame lerp and 1200 ms minimum display duration, then fades (300 ms) into `StartScene`.
  - `game/src/scenes/StartScene.js`: Renders background, `ASTRO KID` title, tagline, animated `astro_idle` and `mentor_idle` sprites, pixel speech bubble with stepped tail, `START TO PLAY` button (with pixel play icon and 200 ms fade to `MissionsScene`), and `SETTINGS` button.
  - `game/src/scenes/SettingsScene.js`: Renders `SETTINGS` panel with 4 rows (`MUSIC` slider, `SOUND FX` slider, `MENTOR TIPS` toggle, `FULLSCREEN` toggle synced to `enterfullscreen`/`leavefullscreen`), `BACK` button, and `Escape` shortcut, persisting changes immediately.
  - `game/src/scenes/MissionsScene.js`: Renders 3x2 mission badge grid (`300x250` aspect-fitted slots), 2-step 6 px hover lift on unlocked slots, `0x4a4a5a` tint + 8x9 pixel padlock + `audio.denied()` blip on locked slots, arrow-key yellow focus outline + `Enter` activation, and `BACK` button.
  - `game/src/scenes/MissionStubScene.js`: Placeholder mission screen with `MISSION {n}`, `Coming soon.`, `COMPLETE (TEST)` button (calls `SaveManager.completeMission(n)` and returns to `MissionsScene`), and `BACK` button.

## How to Run

```bash
cd game
npm install
npm run dev
```

Open `http://localhost:5173` in a browser.

To create a production build in `game/dist/`:

```bash
cd game
npm run build
```

## Anything Not Done

Nothing — all items in `SPEC-01` are implemented, and `game/public/assets/` was left untouched.
