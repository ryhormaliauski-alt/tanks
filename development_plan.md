# Development Plan: Pixel Tank Battle

This document outlines the step-by-step roadmap for developing **Pixel Tank Battle**, a local 1v1 retro top-down browser game built with Phaser 3, Arcade Physics, and Tailwind CSS in a single standalone HTML structure.

---

## Technical Stack & Architecture Strategy
* **Single-File Delivery:** All markup, styling, Phaser engine initialization, and logic reside in `index.html`.
* **Renderer & Physics:** Phaser 3 using WebGL/Canvas renderer and Arcade Physics.
* **UI & Styling:** Tailwind CSS + Google Fonts (`Press Start 2P`). Procedural/canvas-generated pixel-art graphics to keep external asset dependencies minimal.

---

## Phase 1: Engine Setup & Architecture Setup
- [ ] **1.1 HTML Baseline & Asset Pipeline**
  - Create `index.html` including Phaser 3 CDN, Tailwind CSS CDN, and Google Fonts (`Press Start 2P`).
  - Set up layout structure: fixed game canvas container wrapped in a retro arcade border with CRT scanline overlays.
- [ ] **1.2 Asset & Texture Generator Engine**
  - Implement programmatic canvas texture generation for tanks (Light, Medium, Heavy), weapon projectiles, explosions, and environment tiles across four color palettes:
    - *City:* Brick walls, concrete, dark asphalt base.
    - *Factory:* Metal plating, steel crates, iron grid tiles.
    - *Winter Forest:* Snow terrain, ice blocks, pine tree obstacles.
    - *Desert:* Sand dunes, sandstone ruins, rock pillars.
- [ ] **1.3 Game Scene Flow Initialization**
  - Configure Phaser Scene lifecycle: `BootScene`, `MenuScene` (chassis select), `GameScene` (main arena), and `GameOverScene`.

---

## Phase 2: Chassis Selection & Tank Movement Controls
- [ ] **2.1 Player Chassis Selection Screen (`MenuScene`)**
  - Build UI for P1 and P2 to independently pick their Tank Chassis Class:
    - **Light Tank:** 60 HP | 210 px/s forward | 190 deg/s turn rate.
    - **Medium Tank:** 100 HP | 150 px/s forward | 140 deg/s turn rate.
    - **Heavy Tank:** 160 HP | 95 px/s forward | 90 deg/s turn rate.
- [ ] **2.2 Dual-Player Keyboard Mapping**
  - Bind shared keyboard controls:
    - **Player 1:** Steer (`A`/`D`), Drive (`W`/`S`), Shell (`F`), Missile (`G`).
    - **Player 2:** Steer (`Left`/`Right`), Drive (`Up`/`Down`), Shell (`K`), Missile (`L`).
- [ ] **2.3 Classic Tank Physics Engine**
  - Implement rotation-in-place and directional velocity acceleration along current tank heading vector.
  - Set up tank collision bounds and body dimensions calibrated to match chassis size.

---

## Phase 3: Procedural Arena & Destructible Terrain
- [ ] **3.1 Procedural Grid Terrain Generator**
  - Create a grid-based tile map generator that creates balanced spawn points at opposite ends of the arena.
  - Populate destructible obstacle blocks in procedural patterns ensuring open central lines and tactical cover zones.
- [ ] **3.2 Destructible Wall Collision System**
  - Set up Arcade Physics collision layers between tank bodies and tilemap obstacles.
  - Implement health/durability tracking for individual obstacle tiles.

---

## Phase 4: Weapons System & Combat Mechanics
- [ ] **4.1 Primary Weapon: Heavy Shell**
  - High velocity, low-to-medium damage (15–20 HP).
  - Fast cooldown (~0.3s).
  - On hit: Damages target or destroys 1 obstacle tile on direct hit.
- [ ] **4.2 Secondary Weapon: Missile**
  - Slow velocity, high damage (40–50 HP).
  - Slow cooldown (~1.5s).
  - Area-of-Effect (AoE): Explodes in a 3x3 grid radius, destroying multiple obstacle tiles and dealing splash damage.
- [ ] **4.3 Cooldown Timers & Ammo Mechanics**
  - Implement cooldown management per player and per weapon type.

---

## Phase 5: Match Rules, Round Lifecycle & HUD
- [ ] **5.1 HUD & Status Indicators**
  - Render real-time HTML/Phaser overlay HUD:
    - P1 and P2 animated dynamic HP progress bars.
    - Match score counter (Best-of-5 rounds).
    - Visual weapon cooldown meters for Shell and Missile.
- [ ] **5.2 Match Rule Engine & Win Conditions**
  - Track round wins; first player to 3 round victories wins the match.
  - Reset scene with a new terrain theme and newly generated map upon round conclusion.
- [ ] **5.3 Game Over & Replay Screen**
  - Display winner banner with arcade victory animations.
  - Provide options for "Rematch" (same chassis) or "Change Tanks" (return to `MenuScene`).

---

## Phase 6: Visual Polish, Audio & Optimization
- [ ] **6.1 Juice & Visual Effects**
  - Add screen shake on missile explosions.
  - Implement particle smoke trails for missiles and dynamic explosion fragments when destructible blocks break.
  - Apply optional canvas CRT scanline filter overlay.
- [ ] **6.2 Web Audio Synthesis**
  - Use Web Audio API to create retro 8-bit sound effects: engine hums, shell fire, missile launch, explosion booms, and round end chimes.
- [ ] **6.3 Final QA & Self-Contained Testing**
  - Ensure zero external image assets required (100% standalone single-file `index.html`).
  - Verify smooth 60 FPS performance and control responsiveness across modern desktop browsers.