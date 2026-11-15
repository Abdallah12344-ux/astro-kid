# Astro Kid — Lunar Outpost

**Astro Kid** is a 2D educational space-exploration and resource-science web game built with **Phaser 3** and **Vite**. Step onto the Moon with your companion drone **NOVA**, inspect your temporary lunar outpost, explore surface plains and underground lava-tube caves, collect real lunar rock samples (regolith, ilmenite-bearing basalt, and silicate-rich rock), and research sustainable In-Situ Resource Utilization (ISRU) technologies.

## Gameplay & Presentation Videos

- **Full Level 01 Gameplay:** [`presentation/AstroKid_Level1_Gameplay_Full.mp4`](./presentation/AstroKid_Level1_Gameplay_Full.mp4)
- **Project Presentation:** [`presentation/AstroKid_Presentation.mp4`](./presentation/AstroKid_Presentation.mp4)

## Controls

| Action | Default Keys |
| --- | --- |
| Move | `W` `A` `S` `D` / Arrow Keys |
| Interact / Advance Dialogue | `E` |
| Mine Rock (with Pickaxe equipped) | Left Mouse Click |
| Toggle Flashlight | `F` |
| Hotbar Slots (`1`–`9`) | `1`–`9` or Mouse Wheel |
| Backpack | `I` |
| Lunar Map | `M` |
| Technology Tree (inside Outpost) | `C` |
| Toggle Mission Objective Panel | `Tab` |
| Pause / Close Menu | `Esc` |

*(All controls can be customized in the in-game **Settings** menu.)*

## Local Development

```bash
# Install dependencies
npm run install:game

# Start local dev server (http://localhost:5173)
npm run dev

# Build production bundle (outputs to game/dist)
npm run build

# Preview production build locally
npm run preview
```

## Deploying to Vercel

This repository includes ready-to-deploy `vercel.json` configurations at both the repository root and `game/`:

1. Push this repository to **GitHub**.
2. Go to [vercel.com/new](https://vercel.com/new) and import the GitHub repository.
3. Click **Deploy** — no manual build settings are needed (`vercel.json` automatically installs `game` dependencies, runs `vite build`, serves `game/dist`, and enables immutable caching for `/assets/*` and `/audio/*`).
