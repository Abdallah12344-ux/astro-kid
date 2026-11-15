# DONE — SPEC 02: Level 1 World, Player Movement, Input Actions, Audio Manager

## What Was Built

Implemented all features specified in `specs/SPEC-02-world-and-player.md`:

- **World & Physics Constants (`game/src/core/constants.js`)**
  - Exported exact world, physics, zone, airlock, and rock cluster constants (`VIEW_W`, `VIEW_H`, `WORLD_W`, `GROUND_Y`, `GROUND_H`, `GRAVITY`, `JUMP_V`, `WALK_SPEED`, `RUN_SPEED`, `ACCEL`, `FRICTION`, `PLAYER_SCALE`, `PLAYER_START_X`, `ZONES`, `AIRLOCK_X`, `ROCK_CLUSTERS_X`).
- **Save Updates (`game/src/save.js`)**
  - Added `voice: 80` and `keys: DEFAULT_KEYS` to `settings`, backfilling missing keys/fields when loading older `astrokid.v1` saves.
- **Input Actions (`game/src/core/actions.js`)**
  - Implemented `init(scene)`, `isDown(name)`, `justDown(name)`, `justUp(name)`, `rebind(name, slot, keyName)` (with `{ conflict: '<otherAction>' }` detection), `swap(a, slotA, b, slotB)`, `reset()`, and left-mouse-button support for `useTool` using `scene.input.keyboard.addKey(...)`.
- **AudioManager v2 (`game/src/core/audio.js`, replacing `game/src/audio.js`)**
  - Built Phaser SoundManager-backed `audio` singleton with `music`, `sfx`, `voice`, and `amb` buses; browser-autoplay unlock queue for `music()` and `loop()`; `preload()`, `play()`, `playVariant()` (no consecutive repeats), `loop()`, `stop()`, `music()` crossfade/no-op, `duck()`/`unduck()`, `setVolumes()`, `click()`, `denied()`, and `stopAll()`.
  - Updated all imports across the project and removed `game/src/audio.js`.
- **Settings Scene Updates (`game/src/scenes/SettingsScene.js`)**
  - Added the `VOICE` slider row (default `80`) between `SOUND FX` and `MENTOR TIPS`, expanded the panel height to `504` px, shifted the toggle rows down by `96` px, and wired live `audio.setVolumes(music, sfx, voice)` updates.
- **Player Entity (`game/src/entities/Player.js`)**
  - Implemented `Player extends Phaser.Physics.Arcade.Sprite` (`origin(0.5, 1)`, scale `0.85`, Arcade body `60x150` at offset `(32, 15)`, `GRAVITY = 650`), walk/run acceleration & friction, variable-height jump (`JUMP_V = -520` with 0.5x upward release cut), `idle` / `walk` / `air` state machine with held `astro_jump` frames (`0` rising, `1` apex, `2` falling), landing SFX after >0.15 s airborne, rate-limited footstep variants on walk frames `0` and `2`, `canMove` / `facing` properties, and ground shadow ellipse.
- **Level 1 Scene & Menu Wiring (`game/src/scenes/LevelScene.js`, `game/src/scenes/MissionsScene.js`, `game/src/main.js`)**
  - Registered Arcade Physics (`gravity.y = 0`) and `LevelScene` in `main.js`.
  - Wired Mission 1 in `MissionsScene` to fade out (250 ms) into `LevelScene` (`{ mission: 1 }`), while Missions 2–6 continue opening `MissionStubScene`.
  - Built `LevelScene` with parallax `moon_background` and `bg_base` crossfade (`cx` 1700..2000), decorative rock clusters at `[2000, 2450, 2900, 3350]`, static ground collider at `y = 600`, foreground `ground_tile` strip, deadzone camera follow, optional `?debug` overlay, `window.__astro` hook, `MUS_Lunar_Exploration` + `AMB_Lunar_Surface`, and `ESC` return to `MissionsScene` stopping level audio cleanly on shutdown while menu scenes play `MUS_Main_Menu`.

## How to Run

```bash
cd game
npm run dev
```

Open `http://localhost:5173` (or `http://localhost:5173/?debug` for the debug overlay), click **START TO PLAY**, and select **Mission 1** to enter `LevelScene`.

## Anything Not Done

Nothing — all items in `SPEC-02` are implemented, and `game/public/assets/` and `game/public/audio/` were left untouched.
