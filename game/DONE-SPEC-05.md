# DONE — SPEC 05

Implemented **SPEC 05 — Base sprite fix, bottom HUD, inventory, hotbar, backpack, flashlight, pickaxe mining, objectives 04 to 06**:

1. **Outpost HD Sprite Fix (`src/scenes/LevelScene.js`)**
   - Replaced `base_exterior.png` with `assets/level01/world/base_exterior_hd.png` (`1520x963`), applied `Phaser.Textures.FilterMode.LINEAR` to `base_ext` while preserving `NEAREST` on rocks, decals, and astronaut sprites, and rendered at `760 x (760 * 963 / 1520)` with unchanged dome collision and door coordinates.

2. **Item Definitions, Persisted State & Inventory (`src/data/items.js`, `src/core/state.js`, `src/core/inventory.js`, `src/entities/Player.js`)**
   - Added exact `ITEMS` and `SAMPLE_TYPES`.
   - Extended `state` (`astrokid.l1.v1`) with `bag[24]`, `hotbar[9]`, `selected`, `pickaxe`, `flashlight`, `vitals`, `rockHits`, legacy `inventory` save migration, and read-only `state.inventory` live getter.
   - Implemented `inventory` (`add`, `remove`, `count`, `use`, `move`, `selectedItem`), shared `floatText(scene, x, y, text, color)` (`depth 130`), and player stamina drain (`12/s` running) / recovery (`8/s` idle or walking, `15` exhaustion unlock threshold).

3. **Bottom HUD, Sample Counters & Backpack Screen (`src/ui/BottomHud.js`, `src/ui/BackpackPanel.js`, `src/core/actions.js`)**
   - Built `BottomHud` (`depth 100`, `scrollFactor 0`) in both `LevelScene` and `InteriorScene`: portrait, `O2`/stamina/hunger vertical gradient tubes with `<25` red blink, 3 pixel hearts, 9-slot hotbar (`1`–`9`, wheel, click, durability/charge bars, `ON` tag), backpack button (`I` + used-slots badge), and Objective 06 sample counters panel under `ObjectivePanel`.
   - Built `BackpackPanel` (`depth 150`): `6x4` grid with read-only Flashlight/Pickaxe tool cells (`In hotbar`) + bag stacks, pointer drag-and-drop (`inventory.move`), item detail panel with `Durability`/`Battery` bars and `Use`/`Equip`/`Equipped` buttons, and `durability_viewed` trigger.

4. **Tools, Flashlight, Pickaxe Mining & Objectives 04–06 (`src/systems/Tools.js`, `src/entities/Rock.js`, `src/systems/Mining.js`, `src/scenes/InteriorScene.js`)**
   - Implemented `Tools`: held-item icon next to player, `pickaxe_equipped` + `obj04_nova2`/`obj04_astronaut2` trigger, and `F` flashlight toggle with `1 pt / 6 s` battery drain, `<=25` warning & blink, and additive 6-triangle `56°` `320 px` beam + `90 px` circle glow.
   - Implemented `Rock` and `Mining`: `Mine rock` prompt within `140 px`, `objectiveIndex >= 4` gate, `450 ms` swing + crescent arc, `220 ms` impact with durability cost (`1` regolith, `2` iron/silicate), rock shake, debris particles, `+1` sample pickup + `+10` score up to 5 per type, crack/break states, durability warnings (`<=10` and `0`), and Workstation repair (`durability <= 12` -> `30`).
   - Wired Objective 04 25s hint (`hint_obj04`), Objective 05 dialogue (`talked_obj05`), Objective 06 rock-field entry (`x >= 3000`, `talked_obj06`), Objective 06 completion dialogue (`talked_obj06c`), and all `window.__astro` test hooks (`inventory`, `tools`, `mining`, `giveItem`, `setDurability`, `mineNearest`, `openBackpack`).
