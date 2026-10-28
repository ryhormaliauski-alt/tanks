# Product Requirements Document (PRD): Standalone Mode for Pixel Tank Battle

## 1. Overview & Objectives
* **Goal**: Introduce a dedicated Single-Player mode where a human player fights against computer-controlled squads of enemy tanks in progressively escalating rounds.
* **Scope**:
  * Main menu selection between **Local 1v1** and **Standalone / Single Player**.
  * Player chassis selection before starting a run.
  * Round-based enemy progression: enemy count, chassis difficulty, and overall challenge scale upward with higher rounds/levels.
  * Implementation of AI movement, weapon firing, and friendly-fire "aggro switch" retaliation mechanics.

---

## 2. Menu & Mode Selection Updates

### 2.1 Main Menu Scene (`MenuScene`)
* **Mode Selection**: Present two clear options from the main menu:
  1. **Local 1v1 Battle** (Existing two-player split controls).
  2. **Standalone / Single Player** (New single-player round progression vs. AI).
* **Player Configuration**: Standard chassis picker (Light, Medium, Heavy) for the player tank before starting the run.
* **Controls Guidance**: Clear display of solo control schemes (WASD/Arrows to drive, F/G for weapons).

---

## 3. Dynamic Enemy Spawning & Round Progression

### 3.1 Scaling Enemy Count per Round
The number of enemy tanks spawned at the start of a round increases as the player advances through levels:
* **Round 1**: 1 Enemy Tank
* **Round 2**: 2 Enemy Tanks
* **Round 3+**: 3 Enemy Tanks (Maximum density cap per round to maintain arena performance and playability)

### 3.2 Tier Progression & Weighted Chassis Probability
Enemy composition scales dynamically with round progression, introducing lighter tanks first and gradually shifting toward heavier, tougher units in higher rounds:

* **Round 1 (Tier 1 - Light Focus)**:
  * Light Tank: 100%
  * Medium Tank: 0%
  * Heavy Tank: 0%
* **Round 2 (Tier 2 - Light & Medium Introduction)**:
  * Light Tank: 60%
  * Medium Tank: 40%
  * Heavy Tank: 0%
* **Round 3 (Tier 3 - Balanced All Types)**:
  * Light Tank: 30%
  * Medium Tank: 40%
  * Heavy Tank: 30%
* **Round 4+ (Tier 4 - End Game Heavy Focus)**:
  * Light Tank: 10%
  * Medium Tank: 30%
  * Heavy Tank: 60%

### 3.3 Spawn Positioning & Safety Constraints
* **Safe Placement**: Enemy tanks spawn simultaneously at random, non-overlapping positions on the far side of the map (away from the player's initial spawn point).
* **Collision Check**: Coordinates are verified against destructible walls (`wallsGroup`) and existing active entities to prevent immediate overlap.

---

## 4. AI Behavior & Combat Mechanics

### 4.1 Steering & Firing Systems
* **Target Pursuit**: Enemy tanks use simplified steering algorithms to constantly track player coordinates while navigating round obstacle walls.
* **Aggression & Firing**: Enemies automatically fire shells and missiles when aligned within sight lines and out of weapon cooldowns.

### 4.2 Friendly Fire & Infighting ("Aggro Switch" System)
* **Default Focus**: Enemies prioritize attacking the player. They do not intentionally target friendly units.
* **Aggro Shift**: If Enemy A is damaged by Enemy B (via direct shell hit or missile splash damage), **Enemy A's target immediately switches to Enemy B**.
* **Retaliation Persistence**: Enemy A will focus exclusively on Enemy B until Enemy B is destroyed. Upon Enemy B's destruction, Enemy A reverts to hunting the player.
* **Chain Reactions**: If a retaliating enemy accidentally hits a third enemy unit (Enemy C), Enemy C turns on its attacker, triggering local infighting while leaving the player an opportunity to maneuver.

---

## 5. Win / Loss & Round Advancement Flow

### 5.1 Round Completion & Escalation
* **Round Victory**: Cleared when all spawned enemy tanks are destroyed. Clearing a round immediately increments the Level/Round counter and advances the player to the next escalating challenge.
* **Player Defeat**: Triggers when the player tank HP hits 0, leading to a Game Over screen showing the highest round achieved.