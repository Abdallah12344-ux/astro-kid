# DONE — SPEC 04

Implemented SPEC 04 (Dialogue system, radio framing, opening manga cutscene, and Level 1 dialogue triggers `obj01`–`obj04a`):

- **`src/data/sfxmap.js`**: Exported `SFX_MAP` mapping all 27 dialogue SFX tags to sound IDs while ignoring unmapped ambient tags (`WIND`, `ENGINE_HUM`, `LUNAR_FOOTSTEPS`, `LUNAR_AMBIENCE`).
- **`src/ui/DialogueBox.js`**: Built the depth-120 HUD dialogue box at `(250, 446)` with rounded white/gold/navy text panel, masked `124x124` speaker portraits (`dlg_karim`, `dlg_nova`, `dlg_astro`), gradient speaker tab with blinking red radio dot + ` · Radio` suffix, blinking `E ▶` completion indicator, and `220 ms` / `200 ms` vertical slide transitions.
- **`src/systems/Dialogue.js`**: Implemented `Dialogue` (`play`, `isPlaying`, `skipAll`) and shared static `Dialogue.playLines(scene, lines, renderFn, opts)` with `VOICE_<id>` playback, `32` chars/sec typewriter synced to voice completion (with reading-time fallback), `120 ms` staggered SFX tags, radio open/beep/close framing, music ducking (`0.35`), NOVA line hops, `E`/click advance, and `ESC` skip-all.
- **`src/scenes/CutsceneScene.js`**: Implemented the 7-step opening cutscene (`cut1`, `cut4`, `cut5`, `cut6`, `cut8`, `cut9`, `cut7`) with aspect-fitted framed panels, `350 ms` crossfades, speech bubble auto-fitting text + gold speaker tags, `uiPanel` subtitle bar for `cut8`/`cut9`, step 2 liftoff countdown/ignition/thrust/camera shake, descent/touchdown shakes, step 7 `MUS_Lunar_Exploration` 2 s volume ramp-in, and `SKIP` / `ESC` skip to `LevelScene`.
- **Save, Menu, Scene Triggers & Test Hooks**:
  - Added `progress.cutsceneSeen` + `SaveManager.markCutsceneSeen()` in `src/save.js` and wired Mission 1 in `src/scenes/MissionsScene.js`.
  - Wired `obj01` (replacing SPEC 03's temporary 3 s timer) and `obj02` in `src/scenes/LevelScene.js`, plus `obj03` and `obj04a` in `src/scenes/InteriorScene.js`.
  - Registered `CutsceneScene` after `MissionsScene`, bound `F10` to launch `CutsceneScene`, and exposed `dialogue`, `playDialogue(ids)`, and `startCutscene()` on `window.__astro` in `src/main.js`.
