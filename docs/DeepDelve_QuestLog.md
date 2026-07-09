# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> A gamified path from zero Unreal experience to a finished, shippable game.
> **Project:** an idle / incremental mining game called **Deep Delve**.
> **Engine:** Unreal Engine 5.8 · **Path:** Blueprints → C++ · **Art:** Synty POLYGON Dungeon Realms.
> **Live tracker:** https://fdvoracek.github.io/deep-delve/

---

## 📜 About this quest line

You'll build **one complete game** across **17 bite-sized quests** — each a single 30–60 minute sitting that ends with a visible change on screen. **Deep Delve** is an idle/incremental miner in a Synty dwarven mine: you **click a rock face** to chip its HP, and when it breaks it **bursts into coins**. Spend them in a **tech tree** to sharpen and auto-swing your pickaxe, land critical hits, **hire animated dwarves** who mine and collect coins for you, upgrade your crew and veins, and **descend** to tougher, richer rock.

You start in **Blueprints** for fast wins, then pivot to **C++** once the systems are real. Architecture is first-class: the player's damage lives on the Player Controller, rocks/pickaxes/miners are Data Assets, clicks travel through a Blueprint Interface, and every upgrade flows through one data-driven tech tree. You even **ship a playable demo** to a friend at Quest 11, well before the finish.

> **Status:** all 17 quests refined.

### The technology stack

| Area | Technology / tool in Unreal | Learn it in |
|---|---|---|
| Engine & editor | [Unreal Engine 5.8 Editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) | Q1 |
| Importing art | [Fab window](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine) | Q1 |
| Interactive world objects | [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) | Q2 |
| Input | [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) | Q3 |
| Decoupled messaging | [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) | Q3, Q8 |
| Player state | [Player Controller / Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q3, Q9 |
| Audio | [Play Sound 2D & Audio Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine) + [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) | Q3, Q17 |
| User interface | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + [Widget Components](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) | Q3, Q4, Q7 |
| Economy state | [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q4 |
| UI events | [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) | Q4, Q14 |
| Data-driven content | [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) | Q5, Q6, Q8, Q16 |
| Scheduling | [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) | Q8, Q13 |
| Animation | [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) + [Anim Notifies](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine) | Q8 |
| Menus & levels | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + Open Level / [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q9 |
| Persistence | [SaveGame](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) | Q10 |
| Shipping | [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) | Q11, Q17 |
| Code | [C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Reflection macros](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) | Q12 |
| Collision | [Collision & Overlap events](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) | Q16 |
| Polish | [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) | Q17 |
| Art assets | [Synty POLYGON Dungeon Realms](https://syntystore.com/products/polygon-dungeon-realms) | All |

### Rank progression
Prospector (0) → Pit Digger (100) → Miner (350) → Pit Foreman (750) → Master Smith (1,300) → Deeplord (2,000 XP)

**Sound plan (minimal, Play Sound 2D):** rock hit + rock broken → **Q3**; coin pickup → **Q4**; crew hit reuses the hit sound → **Q8**; critical hit → **Q13**; collector reuses coin pickup → **Q16**; ambient/music + optional Sound Cue upgrade → **Q17**.

---

## QUEST 1 — First Light 🌱 · ~50–60 min
Install Unreal, create the project, import the Synty pack, and compose your first scene: a small mine, a dwarf, and a static camera framing the shot.

🎮 **After this quest:** Not interactive yet — you press Play and admire a small dwarven mine with a dwarf in it, framed by a fixed camera.

🛠️ **Teaches:** [Unreal Editor & Projects](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) · [Importing assets via Fab](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine).

**Steps:** New Blank Blueprint project (Starter Content off) → import POLYGON Dungeon Realms via Fab → learn editor navigation → build a small mine from Synty tiles, place a dwarf + light, save as `Mine` → add a Camera Actor with **Auto Activate for Player 0**.

🏁 **Milestone:** editor + Synty imported, and your own scene renders on Play. 🧪 **Boss:** find the dwarf miner and a forge prop by name. 🎁 **Reward:** Prospector · +100 XP.
⭐ **Optional — Living mine:** animate the dwarf (idle/sit, or a small patrol).

---

## QUEST 2 — The First Strike ⛏️ · ~40 min
Make the rock interactive: click it, HP drops, at 0 HP it's destroyed and a fresh rock spawns. No coins or depth yet.

🎮 **After this quest:** You can click the rock and watch its HP fall; after ten hits it shatters and a fresh rock instantly replaces it — but nothing is counted yet.

🛠️ **Teaches:** [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) · [Mouse click events](https://dev.epicgames.com/documentation/en-us/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine) · Spawn & Destroy Actor.

**Steps:** `BP_OreVein` (Static Mesh + `HP` = 10 + collision) → Player Controller: Show Mouse Cursor + Enable Click Events → On Clicked → `HP - 1` → Branch `HP <= 0` → Spawn new + Destroy self.

🏁 **Milestone:** click ten times → it vanishes and a new one appears, forever. 🧪 **Boss:** add a second "power" button that removes 5 HP. 🎁 **Reward:** Pit Digger · +150 XP.
⭐ **Optional — Pickaxe cursor** (On Begin/End Cursor Over) · ⭐ **Rock health bar** ([Widget Component](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + Progress Bar).

---

## QUEST 3 — Pick & Player 🔨 · ~50 min
Click power belongs to the player, not the rock. The Player Controller owns `Damage`, Enhanced Input registers the click, a Blueprint Interface delivers the hit, floating damage numbers appear — and the first sounds land.

🎮 **After this quest:** Clicking now feels like mining: every hit lands with a sound and a floating "1", and the rock shatters with a satisfying crack.

🛠️ **Teaches:** [Player Controller](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) · [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [UMG animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Audio — Play Sound 2D](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine).

**Steps:** `BP_MineController` with `Damage` = 1 → `IA_Dig` (Left Mouse) in a Mapping Context → `BPI_Mineable.TakeDamage(Amount)` → On `IA_Dig` do Get Hit Result Under Cursor → send `TakeDamage(Damage)` → `BP_OreVein` implements it: `HP - Amount`, spawn `WBP_DamageNumber`, break/respawn → **🔊 import two short .wav files; Play Sound 2D on the hit (rock hit) and on the break (rock broken).**

🏁 **Milestone:** hits land with sound + a floating number; the rock cracks apart. 🧪 **Boss:** set `Damage` to 5 and confirm break speed, popup, and hit sound all fire together. 🎁 **Reward:** Pit Digger · +200 XP.

---

## QUEST 4 — Pay Dirt 💰 · ~55 min
A destroyed rock bursts into ~10 coins that scatter; hover the mouse to collect them (with a "ching"). A GameInstance holds Coins + Depth, shown on a new HUD.

🎮 **After this quest:** Breaking a rock pays off — coins burst out, you sweep them up with a 'ching', the Coins counter climbs, and Depth ticks up. The core earn loop is alive.

🛠️ **Teaches:** [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · Physics impulse & spawn loops · Cursor-over pickups · [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine).

**Steps:** `BP_Coin` (mesh + collision + Simulate Physics; Add Impulse on spawn; On Begin Cursor Over → `AddCoins(1)` → destroy) → **🔊 Play Sound 2D (coin pickup) on collect** → Player Controller: Enable Mouse Over Events → `BP_GameInstance` with Coins + Depth; rock break spawns 10 coins, `Depth + 1` → `WBP_HUD` listens to `OnCoinsChanged`; add a `FormatNumber` pure function.

🏁 **Milestone:** break → coins burst → sweep them up → Coins climbs, Depth ticks. 🧪 **Boss:** confirm a coin credits exactly once. 🎁 **Reward:** Miner · +200 XP.
⭐ **Optional — Pickup juice:** coin spin + scale-pop.

---

## QUEST 5 — By the Book 📐 · ~50 min
Rocks and pickaxes become Data Assets, so new content is just data — no graph edits.

🎮 **After this quest:** Plays identically for the player, but rocks and pickaxes are now data you can add to in seconds — invisible to players, huge for you.

🛠️ **Teaches:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · data-driven design.

**Steps:** define `DA_RockData` (Mesh, MaxHP, CoinReward) + `DA_PickaxeData` (Mesh, Damage, Speed) → make `DA_CopperVein` + `DA_StonePick` → refactor `BP_OreVein` to spawn from a RockData ref → refactor the controller to equip a PickaxeData → prove it by adding `DA_IronVein` + `DA_SturdyPick`.

🏁 **Milestone:** create a new rock type + pickaxe purely as data, zero graph changes. 🧪 **Boss:** add a third rock type in under two minutes. 🎁 **Reward:** Miner · +200 XP.
⭐ **Optional — Debug hot-swap** the equipped pickaxe.

---

## QUEST 6 — Tech Tree I: the System 🌳 · ~60 min
Design the general tech-tree data model that drives *every* upgrade. Three nodes to start: Sharpen Pickaxe → Iron Pickaxe → Descend. Tested via temporary buttons.

🎮 **After this quest:** You can spend coins through a plain list of buttons — sharpen your pick, buy a better one, and descend to tougher, richer rock. The whole progression works with placeholder UI.

🛠️ **Teaches:** [Data Assets (tech nodes)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Enumerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) · Maps · [Progression design](https://en.wikipedia.org/wiki/Incremental_game).

**Steps:** `DA_TechNode` (NodeId, DisplayName, Description, Icon, Cost, Prerequisites, Effect = EffectType enum + payload, GridPosition, bRepeatable) → `NodeLevels` map on the GameInstance; availability = prerequisites ≥ 1 and not maxed → `Buy(node)`: gold ≥ cost (repeatable = `Cost × Growth^level`) → spend → `level++` → apply-effect switch → broadcast `OnTechChanged` → author 3 nodes → test with temporary buttons.

🏁 **Milestone:** buying the three nodes sharpens, equips, and descends. 🧪 **Boss:** add a 4th node as data + one enum case. 🎁 **Reward:** Pit Foreman · +300 XP.

---

## QUEST 7 — Tech Tree II: the Screen 🖥️ · ~60 min
Build the real tech-tree screen: nodes placed from data, icons, connector lines, and locked / available / owned colours.

🎮 **After this quest:** Those upgrades move onto a real tech-tree screen — connected nodes with icons that light up when affordable and check off when bought.

🛠️ **Teaches:** [UMG layout & Canvas Panel](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Dynamic widgets](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine).

**Steps:** `WBP_TechNode` (icon, name, cost, state colour) → `WBP_TechTree` loops nodes → Create Widget at `GridPosition` → draw connector lines from prerequisites (OnPaint DrawLine) → click an affordable node to `Buy`; refresh on `OnTechChanged`.

🏁 **Milestone:** a real tech-tree screen — nodes light up when affordable, check off when owned. 🧪 **Boss:** add a 4th node's position + prerequisite, no layout code. 🎁 **Reward:** Pit Foreman · +300 XP.
⭐ **Optional — Pan & zoom.**

---

## QUEST 8 — Hire the Crew ⚒️ · ~60 min
A "Hire Dwarf" tech node spawns a visible animated dwarf who mines the rock on a timer, dealing his own upgradeable damage — with damage numbers and the same hit sound as yours.

🎮 **After this quest:** Buy the Hire Dwarf node and a dwarf walks in and mines the rock on his own — swinging, thudding, and breaking rocks while you do other things. The idle half is live.

🛠️ **Teaches:** [Data Assets (miners)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Interface reuse](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine).

**Steps:** `DA_MinerData` + `DA_Dwarf1` → `BP_Dwarf` plays the mine anim, stats from data → looping timer → `TakeDamage(Damage)` + spawn a damage number → **🔊 reuse the rock-hit sound on each dwarf swing** → `HireMiner` node spawns a dwarf; GameInstance holds `CurrentRockActor`.

🏁 **Milestone:** a dwarf mines on his own, breaking rocks hands-free. 🧪 **Boss:** hire two dwarves; both damage the rock independently. 🎁 **Reward:** Master Smith · +300 XP.
⭐ **Optional — Impact-synced hits** via an [Anim Notify](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine).

---

## QUEST 9 — The Front Door 🚪 · ~50–55 min
The game boots into a Main Menu level; New Game starts a fresh mine; Quit exits. An Esc pause menu lets you resume or head back to the menu.

🎮 **After this quest:** The game opens on a title screen — New Game drops you into the mine, and Esc pauses so you can return to the menu or quit.

🛠️ **Teaches:** [UMG menus](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · Open Level & Input Modes ([Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)) · Quit Game.

**Steps:** MainMenu level + `WBP_MainMenu` (New Game, Quit), set as Game Default Map → New Game: reset GameInstance → **Open Level** Mine → Quit: Quit Game node → in the Mine, `WBP_PauseMenu` (Esc): Resume, Quit to Main Menu, Quit Game; Set Input Mode UI Only + **Set Game Paused** while open.

🏁 **Milestone:** boots to a title screen; New Game → mine; Esc pauses. 🧪 **Boss:** start, pause, quit to menu, start again — second run begins fresh (New Game resets the persistent GameInstance). 🎁 **Reward:** Master Smith · +250 XP.
⭐ **Optional — Title art.**

---

## QUEST 10 — Memory Core 💾 · ~55–60 min
Give the menus persistence — a SaveGame system, a working Continue, and Save/Load in the pause menu. Loading rebuilds the world from the saved data.

🎮 **After this quest:** Your run truly persists — quit to desktop, relaunch, hit Continue, and your coins, upgrades, depth, and dwarves are all exactly back.

🛠️ **Teaches:** [SaveGame system](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) — `USaveGame`, Save/Load to Slot, Does Save Exist / Delete Save — and the distinction between saving data vs reconstructing runtime actors.

**Steps:** `SG_DeepDelve` (Coins, Depth, `NodeLevels`, current rock + pickaxe, dwarf count, `LastSavedTime`) → `SaveGame()` (wire to Save button + autosave) → `RebuildWorld()` (set tier + pickaxe, re-spawn dwarves, refresh UI) → `LoadGame()` → **Continue** on the menu (enabled only if a save exists) loads then Open Level Mine; **New Game** also deletes the save slot.

🏁 **Milestone:** quit, relaunch, Continue → everything restored, dwarves included. 🧪 **Boss:** delete the save and confirm a clean first-run works. 🎁 **Reward:** Master Smith · +300 XP.
⭐ **Optional — Multiple save slots.**

---

## QUEST 11 — Ship a Demo 📦 · ~40–50 min
Your game already has the full core loop and saves — package it into a standalone build and send it to a friend for real feedback. Blueprint-only, so it needs no C++ toolchain yet.

🎮 **After this quest:** You have a standalone, double-clickable Deep Delve demo — the full loop with saving — that a friend can play with no Unreal installed.

🛠️ **Teaches:** [Packaging & build configs](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) (Development vs Shipping), testing and distributing a standalone build.

**Steps:** confirm Game Default Map + name/icon → Platforms → Windows → **Package Project** (Development config, keeps logs) → test the build outside the editor (new game, upgrade, save, quit, relaunch, Continue) → zip and share (Drive/WeTransfer); recipient needs no engine.

🏁 **Milestone:** a shippable demo a friend can play. 🧪 **Boss:** run the build on another folder/machine and complete a full play/save/quit/continue cycle. 🎁 **Reward:** Master Smith · +250 XP · 🎉 first shippable build.
⭐ **Optional — Feedback loop:** 3 questions for your friend.

---

## QUEST 12 — Into the Code 🧬 · ~60 min
The `EffectType` switch has grown unwieldy — the honest reason to reach for C++. Refactor the effect system into a polymorphic `UTechEffect` hierarchy, built and run from Rider.

🎮 **After this quest:** Plays the same to the player, but the tech-tree effects now run through C++ effect classes, and adding a new upgrade is a small new class instead of a growing switch.

🛠️ **Teaches:** [Programming with C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) · [UCLASS/UPROPERTY/UFUNCTION reflection](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [GameInstanceSubsystem](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) · building & debugging from the IDE.

**Steps:** install **JetBrains Rider** (free for non-commercial; Windows needs MSVC build tools, macOS Xcode command-line tools); set Editor Preferences → Source Code Editor = Rider → base `UTechEffect` (`UCLASS`, `EditInlineNew` + `Instanced`, `Blueprintable`) with virtual `Apply(context)` → subclasses (`UpgradePickaxe`, `EquipPickaxe`, `Descend`, `HireMiner`) → change `DA_TechNode.Effect` to an instanced `UTechEffect*` → `Buy(node)` becomes `node.Effect->Apply(context)`; optionally move logic into a `UGameInstanceSubsystem` → **build & run from Rider with the debugger (no Live Coding — full builds keep engine state clean and the debugger reliable; close the editor before a full rebuild).**

🏁 **Milestone:** identical gameplay, but each node runs its own C++ effect; you can hit breakpoints. 🧪 **Boss:** add a new `UTechEffect_CoinMultiplier` subclass + node with zero edits to existing effects. 🎁 **Reward:** Deeplord · +300 XP.

---

## QUEST 13 — Sharper & Faster ⚡ · ~55 min
Deepen the pickaxe in the tech tree: damage, an auto-swing (your pick hits on its own, speed-upgradable), and critical hits (a chance for a big-damage, big-flash strike with its own sound).

🎮 **After this quest:** Your pickaxe now swings on its own and occasionally lands a booming, gold "CRIT!" — and you can pour coins into damage, speed, and crit chance.

🛠️ **Teaches:** new C++ [`UTechEffect` subclasses](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [Gameplay Timers (auto-swing)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · random rolls (crits) · [UMG animation + crit sound](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine).

**Steps:** stats `bAutoSwing`, `AutoSwingInterval`, `CritChance`, `CritMultiplier` → auto-swing looping Timer (nodes: Auto-Swing unlock → Swing Speed) → crits: roll `CritChance` per swing, on a crit multiply damage, spawn a gold crit popup, **🔊 play the crit sound** (nodes: Critical Strike → Critical Chance) → extend Sharpen as another damage level → author the nodes.

🏁 **Milestone:** the pick auto-swings and lands flashy crits. 🧪 **Boss:** set `CritChance` to 1.0 (every hit crits) and drop the auto-swing interval (mines hands-free). 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Crit feel:** camera shake + crits multiply coins.

---

## QUEST 14 — The Growing Crew 👥 · ~50–55 min
Buy a bigger crew and spend coins to make every dwarf hit harder and faster — applied live to dwarves already in the mine.

🎮 **After this quest:** The mine fills with dwarves, and upgrading their damage and speed visibly ramps your hands-free income — the whole crew speeds up at once.

🛠️ **Teaches:** global modifiers across live actors · [Event Dispatchers (live update)](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [Data Assets (dwarf types)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine).

**Steps:** `CrewDamageMult` / `CrewSpeedMult` on the subsystem (dwarf damage = `MinerData.Damage × mult`, interval = `Speed ÷ mult`) → new subclasses `UpgradeCrewDamage`, `UpgradeCrewSpeed` → on change, broadcast `OnCrewStatsChanged`; each dwarf re-reads stats and **resets its timer** → ring/line placement → optional new dwarf types.

🏁 **Milestone:** upgrades ramp income; all dwarves speed up at once. 🧪 **Boss:** with several dwarves mining, buy Crew Speed twice and confirm all visibly swing faster. 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Crew cap & formation.**

---

## QUEST 15 — Rich Veins 💎 · ~55 min
Make the veins a progression axis: a yield upgrade that multiplies payouts, new vein tiers to descend into, and a lucky mineral drop — a chance for a broken rock to cough up a gem worth a coin jackpot.

🎮 **After this quest:** Veins get richer as you descend copper → gold → gems, upgrades multiply every payout, and now and then a rock cracks open a gem worth a jackpot of coins.

🛠️ **Teaches:** content via [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) + reused Descend effects · random-chance drops · cursor-over pickup reuse.

**Steps:** vein yield — global `CoinMult` (coins = `RockData.CoinReward × CoinMult`), node `UpgradeYield` → new veins — more `DA_RockData` tiers + Descend nodes chained by prerequisites → lucky mineral — stats `MineralChance`, `MineralValue`; on break roll the chance, spawn `BP_Mineral` (a gem, collected by hover), nodes `UpgradeMineralChance` (+ optional value).

🏁 **Milestone:** richer tiers, multiplied payouts, jackpot gems. 🧪 **Boss:** set `MineralChance` to 1.0, descend a couple tiers, confirm deeper veins pay more. 🎁 **Reward:** Deeplord · +350 XP.
⭐ **Optional — Jackpot feel:** flash + chime; distinct gem meshes per tier.

---

## QUEST 16 — The Coin Collector 🧺 · ~50–55 min
A new dwarf, bought in the tech tree, that walks right-to-left across the mine and auto-collects every coin and mineral it touches. Its Speed is upgradable and you can buy more.

🎮 **After this quest:** A dwarf strolls across the mine hoovering up coins on its own — buy more and upgrade their speed to keep the floor clear while you watch the money roll in.

🛠️ **Teaches:** [Collision & overlap events](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) · actor movement (Tick + delta time) · [Data Assets (collector)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine).

**Steps:** `DA_CollectorData` + `BP_CollectorDwarf` (walk anim + overlap shape) → movement: on Tick move left by `Speed × DeltaTime`; past the left edge, reset to the right → auto-collect: `OnComponentBeginOverlap` with a coin/mineral → call its `Collect()` (refactor the coin's collect into a shared function; reuses the coin-pickup sound) → nodes: Hire Collector + Collector Speed (Q14 live-update applies).

🏁 **Milestone:** the collector auto-sweeps coins as it walks. 🧪 **Boss:** let coins pile up, confirm it sweeps them, then buy a second collector. 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Collector polish:** a filling cart + spline pathing.

---

## QUEST 17 — Strike It Rich 🚀 · ~60 min · *finale*
Take everything you built and make it feel like a real game, then cut a polished v1.0 release. The core SFX are already in — this is animation, VFX, ambience, and the final Shipping build.

🎮 **After this quest:** The finished game — clicks punch, coins ching, crits boom, dust and sparks fly — running as a polished standalone v1.0 you can share.

🛠️ **Teaches:** [UMG Animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Audio & Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging a Shipping build](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project).

**Steps:** UMG animations (DIG punch, purchase pops, unlock flourish) → audio: ambient/music, optionally upgrade the Play-Sound-2D calls to Sound Cues with pitch variation → Niagara: dust on break, sparks on crit, shimmer on mineral, poof on coin collect (+ optional camera shake) → ship v1.0: icon/splash/name, Shipping config, Package, test end-to-end, share.

🏁 **Milestone:** a polished standalone v1.0, playable start to finish. 🧪 **Boss:** the Shipping build plays the whole game with sound + effects. 🎁 **Reward:** Deeplord, Shaper of the Deep · +400 XP · 🏅 QUEST LINE COMPLETE.
⭐ **Optional — Options & publish:** settings menu + itch.io page.

**🎓 What to learn next:** the **Gameplay Ability System (GAS)**, **replication & multiplayer**, and **AI / Behavior Trees** — none needed for Deep Delve, but great directions for game #2.

---

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Docs: dev.epicgames.com*
