# DONE — SPEC 03: Objectives, Interactions, NOVA, Outpost Interior (Objectives 01 to 03)

## What Was Built

Implemented all systems, UI components, companion entity, and interior scene specified in `specs/SPEC-03-objectives-interactions-interior.md`:

- **Font & HUD Theme (`game/index.html`, `game/src/main.js`, `game/src/ui/theme.js`)**
  - Added Google Font `Chakra Petch` (`500`, `600`, `700`) to `index.html` and font readiness wait in `main.js`.
  - Implemented `UI` design tokens and `uiPanel(scene, x, y, w, h)` with 8-band vertical gradient (`navy2` to `navy`), 3 px rounded border (`radius = 10`), 2 px inner outline, and 2 px outer shadow outline.
- **EventBus, GameState & Objectives Data (`game/src/core/events.js`, `game/src/core/state.js`, `game/src/data/objectives.js`)**
  - Created global `bus` (`Phaser.Events.EventEmitter`).
  - Defined all 10 Level 1 objectives in `OBJECTIVES` plus `SCORE_EVENTS`.
  - Built `state` persisted in `localStorage` (`astrokid.l1.v1`, protected with `try/catch`) with `set(conditionId)`, `has(conditionId)`, `addScore(n, reason)`, `setFlag()`, `emitSceneStart()`, `reset()`, automatic objective completion scoring (`+100`) and 1500 ms `objective:start` progression, plus `F9` debug reset-and-reload shortcut across all scenes.
- **HUD & Modal Components (`game/src/ui/ObjectivePanel.js`, `ScorePanel.js`, `InteractPrompt.js`, `InfoCard.js`)**
  - `ObjectivePanel`: Top-left HUD panel with clipboard icon, gold header, dynamic wrapped description, animated condition checkboxes (200 ms pop + `SFX_Objective_Updated`), green border flash + `+100` floating text + `SFX_Objective_Complete` on completion, 300 ms `Back.easeOut` slide-in on `objective:start`, and `O` key slide toggle.
  - `ScorePanel`: Top-center HUD panel with 34x34 pixel gold star, live 500 ms score count-up animation, and 60 ms `SFX_Score_Tick`.
  - `InteractPrompt`: World-positioned prompt above the nearest active interactable with 28x28 gradient gold key badge, 120 ms fade-in, and first-sight `SFX_Interaction_Prompt`.
  - `InfoCard`: Modal dialog (`640x330` at depth 150 with dimmed backdrop) pausing player movement, playing `SFX_Menu_Open` / `SFX_Menu_Close`, and closing on `interact` (`E`) or `ESC` without exiting the scene.
- **Interactions & Companion NOVA (`game/src/systems/Interactions.js`, `game/src/entities/Nova.js`)**
  - `Interactions`: Nearest-in-range interactable resolver triggering `SFX_Interact` and `onInteract()` on `interact` justDown.
  - `Nova`: Floating companion (`nova_victory.png` frame 0, scale `0.8`, depth `11`, no frame/glow) following behind/above the player (`0.06` ground lerp, `0.04` air lag, sinusoidal hover bob), looping `SFX_NOVA_Idle_Hum`, with `nova.say(text, ms)` speech bubble support.
- **Audio, Player Surfaces, LevelScene & InteriorScene (`game/src/core/audio.js`, `game/src/entities/Player.js`, `game/src/scenes/LevelScene.js`, `game/src/scenes/InteriorScene.js`)**
  - Added `audio.playFromList(ids, opts)` and `Player.surface` (`'regolith'` vs `'metal'`) for metal footsteps and landing inside the outpost.
  - Updated `LevelScene` with `ObjectivePanel`, `ScorePanel`, `Interactions`, `Nova`, temporary 3000 ms Objective 01 radio trigger, airlock interaction at `x = 1400` (`Airlock_Unlock` -> `Airlock_Open` -> 700 ms fade -> `airlock_entered` -> `InteriorScene`), and return spawn at `x = 1440` with 500 ms fade-in and `Airlock_Close`.
  - Created `InteriorScene` (`1280x720` fixed camera) with `cabin` backdrop, airlock door, 4 inspectable stations (`workstation`, `storage`, `lifesupport`, `bed`), ghost processor install spot, persistent gold check marks at `(x, 330)` backed by `inspected_<id>` flags, interior music/ambience/`SFX_LifeSupport_Hum`, and exit door back to `LevelScene`.
  - Extended `window.__astro` with `state`, `bus`, `objectives`, `go(sceneKey, data)`, and `inspectAll()`.

## How to Run

```bash
cd game
npm run dev
```

Open `http://localhost:5173`, start **Mission 1**, wait 3 seconds for Objective 01, walk right to the airlock at `x = 1400` and press `E` to enter the outpost, then inspect all four stations with `E` (press `F9` anytime to reset Level 1 progress).

## Anything Not Done

Nothing — all items in `SPEC-03` are implemented, and `game/public/assets/` and `game/public/audio/` were left untouched.
