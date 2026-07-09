# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> A gamified path from zero Unreal experience to a finished, shippable game.
> **Project:** an idle / incremental mining game called **Deep Delve**.
> **Engine:** Unreal Engine 5.8 · **Path:** Blueprints → C++ · **Art:** Synty POLYGON Dungeon Realms.
> **Live tracker:** https://fdvoracek.github.io/deep-delve/

---

## 📜 About this quest line

You'll build **one complete game** across a series of bite-sized quests — each a single 30–60 minute sitting that ends with a visible change on screen. **Deep Delve** is an idle/incremental miner set in a Synty dwarven mine: you **click a rock face** to chip its HP, and when it breaks it **bursts into coins** you sweep up with the mouse. Spend those coins in a **tech tree** to sharpen your pickaxe, buy better picks, **hire animated dwarves** who mine alongside you, and **descend** to tougher, richer rock. The goal is depth — how far down can you dig?

You start in **Blueprints** for fast wins, then pivot to **C++** once the systems are real. Architecture is first-class here: the player's damage lives on the Player Controller, rocks and pickaxes are Data Assets, clicks travel through a Blueprint Interface, and every upgrade flows through one data-driven tech tree.

> **Status:** Quests 1–9 are fully refined; Quests 10–12 are drafts still being designed.

### The technology stack

| Area | Technology / tool in Unreal | Learn it in |
|---|---|---|
| Engine & editor | [Unreal Engine 5.8 Editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) | Q1 |
| Importing art | [Fab window](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine) | Q1 |
| Interactive world objects | [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) | Q2 |
| Input | [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) | Q3 |
| Decoupled messaging | [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) | Q3, Q8 |
| Player state | [Player Controller / Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q3 |
| UI | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + [Widget Components](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) | Q3, Q4, Q7 |
| Economy state | [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q4 |
| UI events | [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) | Q4 |
| Data-driven content | [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) | Q5, Q6, Q8 |
| Scheduling | [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) | Q8 |
| Animation | [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) + [Anim Notifies](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine) | Q8 |
| Persistence | [SaveGame](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) | Q9 |
| Code | [C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) + [Live Coding](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime) | Q10 |
| Polish & shipping | [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) | Q11–Q12 |

### Rank progression
Prospector (0) → Pit Digger (100) → Miner (350) → Pit Foreman (750) → Master Smith (1,300) → Deeplord (2,000 XP)

---

## QUEST 1 — First Light 🌱 · ~50–60 min
Install Unreal, create the project, import the Synty pack, and compose your first scene: a small mine, a dwarf, and a static camera framing the shot.

🛠️ **Teaches:**
- **[Unreal Editor & Projects](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation)** — the tool you build everything in: Viewport, Content Browser, PIE.
- **[Importing assets via Fab](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine)** — how the Synty pack gets into your project.

**Steps:** New Blank Blueprint project (Starter Content off) → import POLYGON Dungeon Realms via Fab → learn editor navigation → build a small mine from Synty tiles, place a dwarf + light, save as `Mine` → add a Camera Actor framing the shot with **Auto Activate for Player 0**.

🏁 **Milestone:** editor + Synty imported, and your own scene (mine + dwarf + static camera) renders on Play.
🧪 **Boss check:** find the dwarf miner and a forge prop by name in the Content Drawer.
🎁 **Reward:** Prospector · +100 XP · ☕
⭐ **Optional — Living mine:** animate the dwarf (idle/sit loop, or a small patrol) — [Animation Sequences](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine).

---

## QUEST 2 — The First Strike ⛏️ · ~40 min
Make the rock interactive: click it, HP drops, at 0 HP it's destroyed and a fresh rock spawns. No coins or depth yet.

🛠️ **Teaches:**
- **[Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — `BP_OreVein` holds the rock's mesh + HP.
- **[Mouse click events](https://dev.epicgames.com/documentation/en-us/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine)** — Enable Click Events + the actor's On Clicked.
- **[Spawn & Destroy Actor](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — destroy the broken rock, spawn its replacement.

**Steps:** `BP_OreVein` (Static Mesh + `HP` = 10 + collision) → Player Controller: Show Mouse Cursor + Enable Click Events → On Clicked → `HP - 1` → Branch `HP <= 0` → Spawn new + Destroy self.

🏁 **Milestone:** click the rock ten times → it vanishes and a new one appears, forever.
🧪 **Boss check:** add a second "power" button that removes 5 HP.
🎁 **Reward:** Pit Digger · +150 XP · 🍪
⭐ **Optional — Pickaxe cursor** (On Begin/End Cursor Over) · ⭐ **Optional — Rock health bar** ([Widget Component](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + Progress Bar).

---

## QUEST 3 — Pick & Player 🔨 · ~45 min
Refactor: click power belongs to the player, not the rock. The Player Controller owns `Damage`, Enhanced Input registers the click, a Blueprint Interface delivers the hit, and floating damage numbers appear.

🛠️ **Teaches:**
- **[Player Controller](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)** — owns the player's `Damage`.
- **[Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine)** — an Input Action + Mapping Context registers the dig.
- **[Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine)** — `BPI_Mineable.TakeDamage` hits any rock without knowing its class (dwarves reuse it later).
- **[UMG animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine)** — a floating damage number via World→Screen projection.

**Steps:** `BP_MineController` with `Damage` = 1 → `IA_Dig` (Left Mouse) in a Mapping Context → `BPI_Mineable.TakeDamage(Amount)` → On `IA_Dig` do Get Hit Result Under Cursor → send `TakeDamage(Damage)` → `BP_OreVein` implements the interface: `HP - Amount`, spawn `WBP_DamageNumber`, break/respawn. Remove the temporary On Clicked.

🏁 **Milestone:** same loop, but click power comes from the Player Controller via an event, and a "1" floats off the rock each hit.
🧪 **Boss check:** set `Damage` to 5 and confirm break speed *and* popup change together.
🎁 **Reward:** Pit Digger · +200 XP · 🎵

---

## QUEST 4 — Pay Dirt 💰 · ~50–60 min
A destroyed rock bursts into ~10 coins that scatter; hover the mouse to collect them. A GameInstance holds Coins + Depth, shown on a new HUD.

🛠️ **Teaches:**
- **[GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)** — the economy's permanent home (Coins, Depth).
- **[Physics impulse & spawn loops](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — spawn 10 coins and Add Impulse to scatter them.
- **[Cursor-over pickups](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — On Begin Cursor Over collects (needs Enable Mouse Over Events).
- **[Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine)** — `OnCoinsChanged` refreshes the HUD.

**Steps:** `BP_Coin` (mesh + collision + Simulate Physics; Add Impulse on spawn; On Begin Cursor Over → `AddCoins(1)` → destroy) → Player Controller: Enable Mouse Over Events → `BP_GameInstance` with Coins + Depth → rock break spawns 10 coins, `Depth + 1` → `WBP_HUD` listens to `OnCoinsChanged`; add a `FormatNumber` pure function (1.2K).

🏁 **Milestone:** break a rock → coins burst → sweep the mouse over them → Coins climbs, Depth ticks up.
🧪 **Boss check:** confirm a coin credits exactly once and the HUD matches.
🎁 **Reward:** Miner · +200 XP · 🍕
⭐ **Optional — Pickup juice:** coin spin, a "ching" [Sound Cue](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine), scale-pop.

---

## QUEST 5 — By the Book 📐 · ~50 min
Rocks and pickaxes become Data Assets, so new content is just data — no graph edits.

🛠️ **Teaches:**
- **[Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)** — `DA_RockData` (Mesh, MaxHP, CoinReward), `DA_PickaxeData` (Mesh, Damage, Speed).
- **Data-driven design** — separate stats (data) from behavior (Blueprints).

**Steps:** define the two Data Asset types → make `DA_CopperVein` + `DA_StonePick` (current values as data) → refactor `BP_OreVein` to spawn from a RockData ref → refactor the controller to equip a PickaxeData (damage = `Pickaxe.Damage + upgrades`) → prove it by adding `DA_IronVein` + `DA_SturdyPick` and swapping refs.

🏁 **Milestone:** create a new rock type + pickaxe purely as Data Assets and drop them in, zero graph changes.
🧪 **Boss check:** add a third rock type in under two minutes, data only.
🎁 **Reward:** Miner · +200 XP · 🚶
⭐ **Optional — Debug hot-swap** the equipped pickaxe at runtime.

---

## QUEST 6 — Tech Tree I: the System 🌳 · ~60 min
Design the general tech-tree data model that will drive *every* upgrade — pickaxe, descend, hire crew, and all future nodes. Three nodes to start: Sharpen Pickaxe → Iron Pickaxe → Descend. Tested via temporary buttons.

🛠️ **Teaches:**
- **[Data Assets (tech nodes)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)** — `DA_TechNode`: `NodeId`, `DisplayName`, `Description`, `Icon`, `Cost`, `Prerequisites`, `Effect`, `GridPosition`, `bRepeatable`.
- **[Enumerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — `EffectType` drives a switch that applies each node.
- **[Maps & data structures](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — `NodeLevels` (NodeId→level) is your whole progression state.
- **[Progression design](https://en.wikipedia.org/wiki/Incremental_game)** *(principle)* — prerequisite gating + rising costs.

**Steps:** `DA_TechNode` with Effect = `EffectType` enum + payload (`EffectValue`, `TargetPickaxe`, `TargetRock`) → `NodeLevels` map on the GameInstance; availability = all prerequisites ≥ 1 and not maxed → `Buy(node)`: gold ≥ cost (repeatable cost = `Cost × Growth^level`) → spend → `level++` → apply-effect switch → broadcast `OnTechChanged` → author 3 nodes → test with a temporary button list.

🏁 **Milestone:** buying the three nodes actually sharpens, equips, and descends.
🧪 **Boss check:** add a 4th node as data + one enum case; prerequisites gate it.
🎁 **Reward:** Pit Foreman · +300 XP · 🍕 (the backbone of the whole game)

---

## QUEST 7 — Tech Tree II: the Screen 🖥️ · ~60 min
Build the real tech-tree screen: nodes placed from data, icons, connector lines, and locked / available / owned colours.

🛠️ **Teaches:**
- **[UMG layout & Canvas Panel](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine)** — place each node from its `GridPosition`.
- **[Dynamic widgets (Create Widget)](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine)** — one `WBP_TechNode` per `DA_TechNode`.

**Steps:** `WBP_TechNode` (icon, name, cost, state colour) → `WBP_TechTree` loops all nodes → Create Widget at `GridPosition` → draw connector lines from prerequisites (OnPaint DrawLine) → clicking an affordable node calls `Buy`; refresh on `OnTechChanged`.

🏁 **Milestone:** a real tech-tree screen — three connected nodes that light up when affordable and check off when owned.
🧪 **Boss check:** add the 4th node's position + prerequisite and confirm it appears wired in, no layout code.
🎁 **Reward:** Pit Foreman · +300 XP · 🎧
⭐ **Optional — Pan & zoom** for when the tree grows.

---

## QUEST 8 — Hire the Crew ⚒️ · ~55–65 min
A "Hire Dwarf" tech node spawns a visible animated dwarf who mines the rock on a timer, dealing his own upgradeable damage with floating numbers.

🛠️ **Teaches:**
- **[Data Assets (miners)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)** — `DA_MinerData`: Mesh, MineAnim, Damage, Speed.
- **[Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine)** — play the looping mining animation.
- **[Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine)** — each dwarf's looping swing.
- **[Interface reuse (BPI_Mineable)](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine)** — dwarves call the same `TakeDamage` door your clicks use.

**Steps:** `DA_MinerData` + `DA_Dwarf1` (Damage 1) → `BP_Dwarf` (skeletal mesh looping the mine anim; Damage/Speed from data) → looping timer → `TakeDamage(Damage)` to the current rock + spawn a damage number → `HireMiner` node effect spawns a dwarf; GameInstance holds a `CurrentRockActor` ref → coins still burst and are swept up (a future "Coin Collector" dwarf node will auto-gather).

🏁 **Milestone:** buy "Hire Dwarf" → a dwarf appears and mines with a swing animation + damage numbers, breaking rocks on his own.
🧪 **Boss check:** hire two dwarves; both independently damage the rock and pop numbers.
🎁 **Reward:** Master Smith · +300 XP · 🏆
⭐ **Optional — Impact-synced hits** via an [Anim Notify](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine) on the swing's impact frame.

---

## QUEST 9 — Memory Core 💾 · ~45–55 min
Persist everything so the mine survives closing the game. Nearly all progress is one thing: the tech-tree node levels.

🛠️ **Teaches:**
- **[SaveGame system](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine)** — a `USaveGame` + Save/Load Game to Slot.

**Steps:** `SG_DeepDelve` (Coins, Depth, `NodeLevels`, CurrentRock, EquippedPickaxe, `LastSavedTime`) → Save = Create Save Game Object → copy state → `LastSavedTime = UtcNow` → Save to Slot → Load = Does Exist → Load → Cast → copy back → refresh → load on Init + autosave timer + manual button.

🏁 **Milestone:** buy nodes, hire dwarves, close the game, reopen — everything is exactly where you left it.
🧪 **Boss check:** delete the save and confirm a clean first-run still works.
🎁 **Reward:** Master Smith · +250 XP · 😴

---

## QUEST 10 — Into the Code 🧬 · ~60 min · 🚧 DRAFT
*Still being designed — provisional.* The C++ pivot, motivated by real pain: the `EffectType` switch is getting unwieldy, so we refactor it into a polymorphic `UTechEffect` hierarchy in C++.

🛠️ **Teaches:** [C++ Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) · [Reflection macros](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) · [Live Coding](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime).

🏁 **Milestone (draft):** adding a node effect becomes a small new C++ class, not another switch case.
🎁 **Reward:** Deeplord · +350 XP

---

## QUEST 11 — Cave-In ♻️ · ~60 min · 🚧 DRAFT
*Still being designed — provisional.* Prestige (collapse the shaft for a permanent bonus) and offline earnings.

🛠️ **Teaches:** [Date & Time (FDateTime/FTimespan)](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Misc/FDateTime) · [Prestige / meta-progression](https://en.wikipedia.org/wiki/Incremental_game) *(principle)*.

🏁 **Milestone (draft):** offline payout on return + a prestige loop that speeds each run.
🎁 **Reward:** Deeplord · +400 XP

---

## QUEST 12 — Strike It Rich 🚀 · ~60 min · 🚧 DRAFT
*Still being designed — provisional.* Polish the feel and package a standalone build.

🛠️ **Teaches:** [UMG Animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project).

🏁 **Milestone (draft):** a standalone Deep Delve build with sound and effects, playable by anyone.
🎁 **Reward:** Deeplord, Shaper of the Deep · +400 XP · 🏅 QUEST LINE COMPLETE

---

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Docs: dev.epicgames.com*
