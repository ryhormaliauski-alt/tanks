# Product Requirement Document (PRD)
## Project Name: Pixel Tank Battle (2D 1v1 Local Browser Game)

---

## 1. Project Overview & Vision
**Pixel Tank Battle** is a retro 2D top-down local 1v1 tank combat game designed for single-keyboard play in web browsers. Built using **Phaser 3**, the game pits two players against each other in round-based skirmishes across procedurally generated, fully destructible pixel-art arenas.

The game mechanics prioritize tactical trade-offs: armor vs. mobility, and rapid-fire precision vs. slow explosive demolition.

---

## 2. Technical Stack & Architecture
* **Target Environment:** Modern Web Browsers (Desktop).
* **Delivery Format:** Single-file standalone HTML (`index.html`) containing UI, styles, and embedded script modules.
* **Game Engine:** **Phaser 3** (Canvas/WebGL renderer, Arcade Physics).
* **Styling & UI:** Tailwind CSS + Google Fonts (`Press Start 2P`).

---

## 3. Game Flow & Match Rules
1. **Selection Screen:** Both players configure their tank chassis class before starting.
2. **Match Format:** **Best-of-5 rounds** (First player to reach 3 round wins wins the match).
3. **Round Lifecycle:**
   - A random terrain theme (*City, Factory, Winter Forest, Desert*) is selected.
   - Destructible obstacles generate in a random grid pattern.
   - Players spawn at opposing ends of the map.
   - Round ends immediately when a player's tank health drops to `0`.
   - Winners are tracked, and a new arena generates for the next round.

---

## 4. Tank Chassis & Customization Matrix

Player mobility and durability scale based on the chosen weight class:

| Chassis Class | Max HP | Forward Speed | Turn Speed | Characteristics |
| :--- | :---: | :---: | :---: | :--- |
| **Light Tank** | 60 HP | High (210 px/s) | Fast (190 deg/s) | Nimble, easy to dodge projectiles, low armor. |
| **Medium Tank**| 100 HP | Medium (150 px/s)| Moderate (140 deg/s)| Balanced balance between survival and mobility. |
| **Heavy Tank** | 160 HP | Low (95 px/s) | Slow (90 deg/s) | Durable tank, high survivability, sluggish handling. |

---

## 5. Combat System & Weapons

* **Driving Model:** Classic tank controls (Rotate in place with Left/Right steering; Drive Forward/Reverse). Weapons fire straight along the current vehicle orientation.
* **Ammunition:** Infinite ammo managed by cooldown timers.

### Weapon Profiles:
1. **Heavy Shell (`Primary Weapon`)**
   - **Velocity:** Fast projectile speed.
   - **Damage:** Low-to-medium (15-20 HP).
   - **Fire Rate:** Fast cooldown (~0.3 seconds).
   - **Terrain Impact:** Destroys 1 obstacle tile on direct hit.

2. **Missile (`Secondary Weapon`)**
   - **Velocity:** Slow projectile speed.
   - **Damage:** High (40-50 HP).
   - **Fire Rate:** Slow cooldown (~1.5 seconds).
   - **Terrain Impact:** Explodes in a 3x3 radius, obliterating multiple obstacles at once to carve open sightlines.

---

## 6. Controls (Shared Keyboard Layout)

| Action | Player 1 | Player 2 |
| :--- | :---: | :---: |
| **Steer Left / Right** | `A` / `D` | `Left Arrow` / `Right Arrow` |
| **Drive Forward / Reverse** | `W` / `S` | `Up Arrow` / `Down Arrow` |
| **Fire Shell** | `F` | `K` |
| **Fire Missile** | `G` | `L` |

---

## 7. Dynamic Environments & Destructible Terrain

Each round selects one of four themes with distinct color palettes and obstacle textures:
* **City:** Asphalt ground, brick walls, concrete barriers.
* **Factory:** Metallic plating, steel crates, industrial barriers.
* **Winter Forest:** Snow terrain, ice blocks, dense pine trees.
* **Desert:** Sand dunes, sandstone ruins, rock pillars.

**Destruction Logic:**
- Obstacles block both tank movement and projectile paths.
- Shells and missiles must be used to destroy cover and create open firing angles.

**Power-Up Drops:**
- Each destroyed wall tile independently has a 10% chance to spawn armor and a 1% chance to spawn a rocket launcher.
- Each round may also place up to two additional visible rocket launcher pickups at random clear positions in the arena. All drops expire after 10–15 seconds and flash during their final 3 seconds.
- Any active tank can collect a launcher, regardless of HP or armor. Collection plays an airplane sound and sends ten rockets, each twice their previous size and taking twice as long to fall, to independent random positions across the arena.
- Each rocket explodes with missile-scale splash damage to nearby tanks—including the collecting tank—and destroys nearby walls.

---

## 8. Interface & Audio-Visual Style
* **Visuals:** Retro 8-bit / 16-bit pixel-art aesthetics with scanline CRT effects. Explosion camera shake uses reduced amplitudes (0.006 for large explosions, 0.003 for smaller explosions) while preserving the existing shake durations.
* **HUD:** Top header displaying P1 & P2 real-time health bars, round score tracker, and weapon cooldown indicators.
* **End Screen:** Match victory banner with options to rematch or re-configure tank chassis.