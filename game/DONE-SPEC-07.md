# DONE — SPEC 07: Movement Polish, Interaction Prompts, Caves, Minimap, Map & Collapsible Objectives

## Summary of Changes

### 1. Movement & Camera Polish
- **`game/src/main.js`**: Enabled `fps: 120` in Arcade physics config (`physics: { default: 'arcade', arcade: { gravity: { y: 0 }, fps: 120 } }`) while keeping `pixelArt: true` and `roundPixels: true`.
- **`game/src/core/constants.js`**: Updated `ACCEL = 2600`, `FRICTION = 3200`, `TURN_ACCEL = 5200`, `DOOR.range = 120`, and `depthOf = (y) => Math.round((y / 30) * 100) / 100`.
- **`game/src/entities/Player.js` & `game/src/entities/Nova.js`**:
  - Normalized diagonal movement via Arcade body velocity only, instant direction reversal using `TURN_ACCEL = 5200` when `dot < 0`, velocity snap `< 12 px/s -> 0`, and independent axis clamping on bounds.
  - Foot collider resized to `40 x 24` at offset `(42, 141)`; surface and cave rock colliders resized to `56 x 22`.
  - Float-preserving visual integer rounding (`Math.round(x)`, `Math.round(y)`) during render for `Player`, `Nova`, and shadows.
  - Dynamic walk animation `frameRate = Clamp(8 * (speed / WALK_SPEED), 6, 12)`, `80 ms` hysteresis below `20 px/s` before switching `walk -> idle`, and facing updates strictly from input direction.
  - Cave footsteps (`surface === 'cave'`) play `SFX_Footstep_Regolith_01..03` at `gain: 0.5` with a `140 ms` echo at `gain: 0.18`.
- **`game/src/core/actions.js`**: Added `resetMovementKeys()` and wired it to modal/dialogue/TechTree closes (`BackpackPanel`, `MapPanel`, `InfoCard`, `Dialogue`, and scene `RESUME`) so held movement keys never queue motion.
- **`game/src/scenes/LevelScene.js` & `game/src/scenes/CaveScene.js`**: Camera follow set to `startFollow(player, true, 0.12, 0.12)` with no deadzone and integer `scrollX`/`scrollY` rounding on `POST_UPDATE` and `FOLLOW_UPDATE`.

### 2. Player-Head Interaction Prompts
- **`game/src/ui/InteractPrompt.js`**: Anchored prompts at `(player.x, player.y - 175)` projected to screen coordinates (`scrollFactor(0)`) so `E` and mining prompts follow just above the astronaut's head.
- **`game/src/systems/Interactions.js`, `game/src/systems/Mining.js`, `game/src/scenes/LevelScene.js`, `game/src/scenes/InteriorScene.js`, `game/src/scenes/CaveScene.js`**: Removed all `promptY` usages; set door and cave entrance/exit ranges to `120` with labels `Enter Outpost`, `Leave Outpost`, `Enter Cave`, `Leave Cave`, and `Mine rock`.

### 3. Surface Regolith & Dark Resource Caves
- **`game/src/core/constants.js` & `game/src/data/objectives.js`**:
  - Replaced `ROCK_NODES` with 6 surface `regolith` nodes and exported `CAVES` (`iron_cave` at `(4500, 1000)` door `(4500, 1060)`; `crystal_cave` at `(6000, 1900)` door `(6000, 1960)`).
  - Updated Objective 06 description to explain surface regolith vs. cave iron-bearing and silicate rocks, and added NOVA's `700 px` cave approach hint (`hint_caves`).
- **`game/src/ui/theme.js` & `game/src/scenes/BootScene.js`**: Generated procedural `512 x 512` `light_radial` and `56 deg` `light_cone` canvas textures and added `registerUi(scene, obj)`.
- **`game/src/scenes/CaveScene.js`, `game/src/entities/Rock.js`, `game/src/core/state.js`**:
  - Created `CaveScene` (`2560 x 1440`) with `cave_floor`, `mulberry32(777)` decals, `cave_wall` headers, dark borders, side boulders, glowing `Leave Cave` arch, 8 persistent `ironrich` or `silicate` rocks (`<caveId>_<index>` in `state.rockHits`), and decorative ore/crystal formations.
  - Implemented `depth 90` `RenderTexture` darkness (`0x02040c`, `0.94`) erasing player glow (`150 px`), `F` flashlight `460 x 460` directional cone + `260 px` halo (with `300 ms` low-battery blink at `<= 25%`), rock/crystal glows (`120 px` / `90 px`), and NOVA glow (`110 px`).
  - Added cave region discovery toast (`+25` score on first entry, `discovered_<caveId>`), dark cave flashlight hint (`hint_cave_dark`), and `AMB_Shadowed_Area` + ducked exploration music transitions.

### 4. Circular Minimap, Modal Map & Collapsible Objectives
- **`game/src/ui/Minimap.js`**: Added `180 px` circular minimap camera at `(1176, 106)` (`zoom 0.06` surface / `0.1` cave) ignoring all registered UI and `depth >= 90` objects, with compass ring, `N` marker, player facing arrow, off-screen outpost/exit pointer + house icon, and `M  Map` button.
- **`game/src/ui/MapPanel.js`**: Added `M` modal map (`1080 x 620` at `(100, 50)`) layering `map_fog` and `GeometryMask`-revealed `map_full` ellipses per discovered region, region markers (house, pickaxe, `?`, pixel padlocks), discovered cave silhouettes, bobbing interpolated yellow player pin, and legend panel. Pressing `M` indoors shows `The map works outside the outpost`.
- **`game/src/ui/ObjectivePanel.js` & `game/src/ui/BottomHud.js`**: Added `28 x 84` right-edge collapse tab (`O` key or click, `250 ms` `Cubic.easeInOut`, `SFX_Tab_Switch`) persisted in `localStorage['astrokid.ui.v1']`, syncing the Objective 06 sample counters panel, gold alert flash + `!` badge while collapsed, and `4 s` temporary auto-expand on new objectives.
- **Test Hooks (`window.__astro`)**: Added `caves`, `enterCave(id)`, `openMap()`, and `toggleObjectives()` across `main.js`, `LevelScene`, `CaveScene`, and `InteriorScene`.
