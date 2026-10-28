# Product Requirement Document (PRD) — Armor Drops Feature
**Project:** Pixel Tank Battle  
**Module:** Dynamic Arena Power-Ups & Drops System  

---

## 1. Feature Overview
The Armor Drop system incentivizes active map exploration and tactical wall destruction. When destructible obstacle tiles are cleared, players have a chance to collect armor power-ups that restore structural health and grant temp/shield armor.

---

## 2. Spawning Mechanics & Lifespan
* **Drop Trigger:** Each destroyed wall tile rolls an independent **10% drop chance**.
  * When a missile destroys multiple wall blocks in an area-of-effect blast, **each** destroyed block independently rolls its 10% chance.
* **Lifespan:** Drops remain active on the arena map for **10 to 15 seconds**.
  * During the final 3 seconds before despawning, the item flashes rapidly to warn players.
* **Visual Styling:** A pulsing golden shield icon (`0xeab308`) floating slightly above the map tile with a background aura.

---

## 3. Pickup Eligibility & Repair Calculations
* **Pickup Detection:** Triggered when a tank's physical bounding box collides/overlaps with the armor drop item.
* **Benefit Pool:** Each pickup grants up to **100 points** total value, capped at a maximum of **100 Armor**.
* **Health & Armor Conversion Order:**
  1. **Repair Base HP:** If the tank is damaged (`HP < Max HP`), the pickup restores HP up to 100%.
  2. **Grant Armor:** Any remaining points from the 100-point pool are applied directly as **Armor**.
  * *Calculation Example:* A tank with 30 HP remaining picks up a drop:
    * Heals **+70 HP** (restores HP to 100/100).
    * Grants **+30 Armor** (100 - 70 = 30 remaining benefit).
* **Full Stack Exception:** If a tank already has **100 HP** and **100 Armor**, it **cannot** pick up the drop. The drop remains on the map so either player can claim it when eligible.

---

## 4. Combat Absorption Mechanics
* **Damage Flow:** Armor absorbs **100% of incoming damage** (shells or missile splash) prior to any damage reducing the tank's core HP.
* **Depletion:** Armor points absorb damage dollar-for-dollar until reduced to `0`. Excess damage beyond available armor bleeds into base HP.

---

## 5. Visual UI & HUD Integration
* **Header HUD:**
  * A **Gold Progress Bar** (`#eab308`) appears directly below each player's main Health bar whenever `Armor > 0`.
  * The background and bar smoothly dynamically scale to reflect `0–100` armor capacity.
* **In-Game World-Space HUD:**
  * **HP Bar:** Always visible anchored below the tank chassis.
  * **Armor Bar:** A secondary thin golden progress bar renders directly below the overhead HP bar whenever the vehicle has active armor.