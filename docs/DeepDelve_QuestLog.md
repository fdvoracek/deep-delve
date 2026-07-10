# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> A gamified path from zero Unreal experience to a finished, shippable game.
> **Project:** an idle / incremental mining game called **Deep Delve**.
> **Engine:** Unreal Engine 5.8 · **Path:** Blueprints → C++ · **Art:** Synty POLYGON Dungeon Realms.
> **Live tracker:** https://fdvoracek.github.io/deep-delve/

---

## 📜 About this quest line

You'll build **one complete game** across **17 bite-sized quests** — each a single 30–60 minute sitting that ends with a visible change on screen. **Deep Delve** is an idle/incremental miner: you **click a rock face** to chip its HP, and when it breaks it **bursts into coins**. Spend them in a **tech tree** to sharpen and auto-swing your pickaxe, land critical hits, **hire animated dwarves** who mine and collect coins for you, upgrade your crew and veins, and **descend** to tougher, richer rock.

You start in **Blueprints** for fast wins, then pivot to **C++** once the systems are real. You even **ship a playable demo** at Quest 11. Every quest lists the tech it teaches (with docs), the **exact nodes/functions** to search for, and gotchas.

> **Status:** all 17 quests refined. **🗺️ Roadmap:** 17 quests ≈ **3–4 weeks** at one sitting/day. Quests 1–11 are Blueprint-only and end in a shippable demo; 12–17 add C++ and content.

### 🧰 Before you start
- A Windows/macOS machine that can run **Unreal Engine 5.8**, with **~100+ GB free disk** and a modern GPU.
- The **Synty POLYGON Dungeon Realms** pack — you download it as a **zipped Unreal Engine project** and **migrate** its Content into your game project (Quest 1). You must own it; **don't redistribute the assets** — keep them out of a public repo (or make the repo private).
- From Quest 12: **JetBrains Rider** (free for non-commercial) + a compiler toolchain (Windows: MSVC build tools; macOS: Xcode command-line tools).
- A **GitHub account** for version control and hosting this tracker.

### The technology stack
| Area | Technology / tool | Learn it in |
|---|---|---|
| Engine & editor | [UE 5.8 Editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) | Q1 |
| Importing art | [Migrate assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/migrating-assets-in-unreal-engine) | Q1 |
| Interactive objects | [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) | Q2 |
| Debugging | [Print String / Blueprint debugging](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-debugging-example-in-unreal-engine) | Q2 |
| Input | [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) | Q3 |
| Decoupled messaging | [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) | Q3, Q8 |
| Player state | [Player Controller / Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q3, Q9 |
| Audio | [Play Sound 2D](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine) + [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) | Q3, Q17 |
| UI | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + [Widget Components](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) | Q4, Q7 |
| Economy state | [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q4 |
| UI events | [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) | Q4, Q14 |
| Data-driven content | [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) | Q5, Q6, Q8, Q16 |
| Scheduling | [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) | Q8, Q13 |
| Animation | [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) + [Anim Notifies](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine) | Q8 |
| Menus & levels | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + Open Level | Q9 |
| Persistence | [SaveGame](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) | Q10 |
| Shipping | [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) | Q11, Q17 |
| Code | [C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Reflection](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) | Q12 |
| Collision | [Collision & Overlaps](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) | Q16 |
| Polish | [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) | Q17 |
| Version control (optional) | [Source Control](https://dev.epicgames.com/documentation/en-us/unreal-engine/source-control-in-unreal-engine) | Q1 |
| Art assets | [Synty POLYGON Dungeon Realms](https://syntystore.com/products/polygon-dungeon-realms) | All |

### Rank progression
Prospector (0) → Pit Digger (100) → Miner (350) → Pit Foreman (750) → Master Smith (1,300) → Deeplord (2,000 XP)

**Sound plan (minimal, Play Sound 2D):** rock hit + rock broken → **Q3**; coin pickup → **Q4**; crew hit reuses the hit sound → **Q8**; critical hit → **Q13**; collector reuses coin pickup → **Q16**; ambient/music + optional Sound Cues → **Q17**.

Each quest below has: **🎮 After** (the visible result), **🛠️ Teaches** (with docs), **🔑 Key nodes** (exact things to search for), **Steps**, **💡 Notes & gotchas** (where relevant), **🏁 Milestone**, **🧪 Boss check**, **🎁 Reward**.

---

## QUEST 1 — First Light 🌱 · ~50–60 min
Install Unreal, create the project, migrate the Synty assets in from their UE project, and compose your first scene: a small mine, a dwarf, and a static camera.

🎮 **After:** you press Play and admire a small dwarven mine with a dwarf in it, framed by a fixed camera.
🛠️ **Teaches:** [Unreal Editor & Projects](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) · [Migrating assets between projects](https://dev.epicgames.com/documentation/en-us/unreal-engine/migrating-assets-in-unreal-engine).
🔑 **Key nodes:** `open the Synty UE project · right-click its Content folder → Asset Actions → Migrate → pick your project's Content · Content Browser search · place Static Mesh Actor · Add → Camera Actor · Auto Activate for Player 0 · Play (Alt+P)`
**Steps:** New Blank Blueprint project (Starter Content off) → **download & unzip the Synty pack (a full UE project) and open it in 5.8** (convert a copy if it's older) → in the Synty project, **right-click the pack's Content folder → Asset Actions → Migrate → choose your DeepDelve `Content` folder** → back in DeepDelve, learn editor navigation → build a small mine, place a dwarf + light, save as `Mine` → add a Camera Actor (Auto Activate for Player 0).
💡 **Notes & gotchas:** the Synty pack is a **UE project, not a Fab item** — you migrate its Content in. If it targets an older engine, opening it in 5.8 makes a converted copy (fine — migrate from that). Keep the migrated assets in their `Synty/POLYGON` folder and don't edit them in place.
🏁 **Milestone:** editor + Synty imported, and your scene renders on Play. 🧪 **Boss:** find the dwarf miner + a forge prop by name. 🎁 **Reward:** Prospector · +100 XP.
⭐ **Optional — Living mine:** animate the dwarf. ⭐ **Optional — Version control:** put the project under [Git with a UE .gitignore](https://dev.epicgames.com/documentation/en-us/unreal-engine/source-control-in-unreal-engine), use Git LFS for large assets, commit after each quest, and keep the repo private if it holds the Synty pack.

---

## QUEST 2 — The First Strike ⛏️ · ~40 min
Make the rock interactive: click it, HP drops, at 0 HP it's destroyed and a fresh rock spawns.

🎮 **After:** click ten times → the rock shatters and a fresh one replaces it, forever — but nothing is counted yet.
🛠️ **Teaches:** [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) · [Mouse click events](https://dev.epicgames.com/documentation/en-us/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine) · Spawn & Destroy Actor · [Debugging (Print String / Output Log)](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-debugging-example-in-unreal-engine).
🔑 **Key nodes:** `Static Mesh Component · integer variable · Branch · SpawnActor from Class · DestroyActor · Enable Click Events · Event OnClicked · Print String · Window → Output Log`
**Steps:** `BP_OreVein` (Static Mesh + `HP` = 10 + collision) → Player Controller: Show Mouse Cursor + Enable Click Events → On Clicked → `HP - 1` → Branch `HP <= 0` → Spawn new + Destroy self.
💡 **Notes & gotchas:** When something doesn't fire, drop a **Print String** (shows on screen) and open the **Output Log** (Window → Output Log). This is your primary debugging loop for the whole project.
🏁 **Milestone:** endless mine-one/get-another loop. 🧪 **Boss:** add a "power" button that removes 5 HP. 🎁 **Reward:** Pit Digger · +150 XP.
⭐ **Optional — Pickaxe cursor** · ⭐ **Rock health bar** ([Widget Component](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine)).

---

## QUEST 3 — Pick & Player 🔨 · ~45 min
Click power belongs to the player, not the rock. Player Controller owns `Damage`, Enhanced Input registers the click, a Blueprint Interface delivers the hit, and the first hit/break sounds land. *(Damage numbers moved to Q4 to keep the ramp gentle.)*

🎮 **After:** every hit lands with a sound and the rock breaks by the Player Controller's Damage, delivered through an interface.
🛠️ **Teaches:** [Player Controller](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) · [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [Audio — Play Sound 2D](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine).
🔑 **Key nodes:** `Input Action (IA_Dig) · Input Mapping Context · Add Mapping Context (BeginPlay) · Get Hit Result Under Cursor by Channel · Blueprint Interface (create) · Does Implement Interface · TakeDamage (message) · Play Sound 2D · GameMode → PlayerController class`
**Steps:** `BP_MineController` with `Damage` = 1 → create a GameMode, set it as the PlayerController class, set the GameMode in Project Settings → Maps & Modes → `IA_Dig` (Left Mouse) in a Mapping Context → `BPI_Mineable.TakeDamage(Amount)` → On `IA_Dig`: Get Hit Result Under Cursor → send `TakeDamage(Damage)`; remove the temporary On Clicked → **🔊 Play Sound 2D on hit and on break.**
💡 **Notes & gotchas:** You **must** set the GameMode (Maps & Modes) or your custom PlayerController is never used and input silently does nothing. Use Input Mode **Game and UI** (set with the HUD later) so world clicks and UI both work — **UI Only** swallows dig clicks.
🏁 **Milestone:** sound + interface-driven damage. 🧪 **Boss:** set Damage to 5 and confirm break speed + sound respond. 🎁 **Reward:** Pit Digger · +200 XP.

---

## QUEST 4 — Pay Dirt 💰 · ~55–60 min
Coins burst from a broken rock; hover to collect (with a "ching"). GameInstance holds Coins + Depth on a HUD — and floating damage numbers arrive here.

🎮 **After:** coins burst out, you sweep them up, a '1' floats off each hit, and the Coins/Depth HUD climbs. The core earn loop is alive.
🛠️ **Teaches:** [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · Physics impulse & spawn loops · Cursor-over pickups · [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [UMG animations (damage numbers)](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine).
🔑 **Key nodes:** `BP_Coin · Simulate Physics · Add Impulse · Enable Mouse Over Events · OnBeginCursorOver · Get Game Instance → Cast To · Event Dispatcher (Create/Call/Bind) · Create Widget · Add to Viewport · Format Text · Project World Location To Screen · Set Life Span`
**Steps:** `BP_Coin` (Simulate Physics; Add Impulse; On Begin Cursor Over → `AddCoins(1)` → destroy) → **🔊 coin-pickup Play Sound 2D** → Player Controller: Enable Mouse Over Events → `BP_GameInstance` (Coins + Depth); rock break spawns 10 coins, `Depth + 1` → `WBP_HUD` via `OnCoinsChanged` + `FormatNumber` → `WBP_DamageNumber` (UMG float+fade) via Project World To Screen.
💡 **Notes & gotchas:** Uncollected coins pile up as leaked actors — give `BP_Coin` a **Life Span** (e.g. 30s) so stragglers clean themselves up.
🏁 **Milestone:** break → burst → collect → counters climb + floating numbers. 🧪 **Boss:** confirm a coin credits exactly once. 🎁 **Reward:** Miner · +200 XP.
⭐ **Optional — Pickup juice.**

---

## QUEST 5 — By the Book 📐 · ~50 min
Rocks and pickaxes become Data Assets — new content is just data.

🎮 **After:** identical for the player, but rocks/pickaxes are now data you can add to in seconds.
🛠️ **Teaches:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · data-driven design.
🔑 **Key nodes:** `Blueprint Class → Data Asset (PrimaryDataAsset) · create Data Asset instances · object reference variable · Set Static Mesh (from data)`
**Steps:** define `DA_RockData` + `DA_PickaxeData`; make `DA_CopperVein` + `DA_StonePick` → refactor `BP_OreVein` to spawn from a RockData ref → controller equips a PickaxeData → prove it by adding `DA_IronVein` + `DA_SturdyPick`.
🏁 **Milestone:** new rock/pickaxe purely as data, no graph changes. 🧪 **Boss:** add a third rock type in under two minutes. 🎁 **Reward:** Miner · +200 XP.
⭐ **Optional — Debug hot-swap.**

---

## QUEST 6 — Tech Tree I: the System 🌳 · ~60 min
The general tech-tree data model that drives *every* upgrade. Three nodes: Sharpen → Iron Pickaxe → Descend. Tested via temporary buttons.

🎮 **After:** spend coins through a button list — sharpen, buy a pickaxe, descend. The whole progression works with placeholder UI.
🛠️ **Teaches:** [Data Assets (tech nodes)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Enumerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) · Maps · [Progression design](https://en.wikipedia.org/wiki/Incremental_game).
🔑 **Key nodes:** `Blueprint Enumeration · Map variable (Name→int) · Switch on Enum · array of object refs (Prerequisites) · custom Buy() function · Event Dispatcher OnTechChanged`
**Steps:** `DA_TechNode` (Effect = EffectType enum + payload) → `NodeLevels` map; availability = prerequisites ≥ 1 & not maxed → `Buy(node)` (cost = `Cost × Growth^level`) → apply-effect switch → broadcast `OnTechChanged` → author 3 nodes → test with temporary buttons.
💡 **Notes & gotchas:** ⚖️ **Balancing lives here.** `CostGrowth` (~1.10–1.15) and each node's Cost/EffectValue set the whole game's pacing. Start gentle — small exponent changes have huge effects — and plan to tune after the demo.
🏁 **Milestone:** buying the 3 nodes sharpens, equips, and descends. 🧪 **Boss:** add a 4th node as data + one enum case. 🎁 **Reward:** Pit Foreman · +300 XP.

---

## QUEST 7 — Tech Tree II: the Screen 🖥️ · ~60 min
The real tech-tree screen: nodes from data, icons, connector lines, state colours.

🎮 **After:** a real tech-tree screen — nodes light up when affordable, check off when bought.
🛠️ **Teaches:** [UMG layout & Canvas Panel](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Dynamic widgets](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine).
🔑 **Key nodes:** `Canvas Panel · Create Widget (in a loop) · Add Child to Canvas · Slot as Canvas Slot → Set Position · OnPaint → Draw Line · SetColorAndOpacity · SetIsEnabled`
**Steps:** `WBP_TechNode` (icon, name, cost, state colour) → `WBP_TechTree` loops nodes → Create Widget at `GridPosition` → draw connector lines from prerequisites (OnPaint) → click affordable node → `Buy`; refresh on `OnTechChanged`.
🏁 **Milestone:** the visual tree you play with. 🧪 **Boss:** add a 4th node's position + prerequisite, no layout code. 🎁 **Reward:** Pit Foreman · +300 XP.
⭐ **Optional — Pan & zoom.**

---

## QUEST 8 — Hire the Crew ⚒️ · ~60 min
A "Hire Dwarf" node spawns an animated dwarf who mines on a timer, with damage numbers and the same hit sound as yours.

🎮 **After:** a dwarf walks in and mines the rock on his own while you do other things. The idle half is live.
🛠️ **Teaches:** [Data Assets (miners)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Interface reuse](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine).
🔑 **Key nodes:** `Skeletal Mesh Component · Anim → Use Animation Asset (loop) · Set Timer by Function Name (looping) · SpawnActor (dwarf) · TakeDamage (interface) · GameInstance CurrentRockActor reference`
**Steps:** `DA_MinerData` + `DA_Dwarf1` → `BP_Dwarf` plays the mine anim → looping timer → `TakeDamage(Damage)` + spawn a damage number → **🔊 reuse the rock-hit sound** → `HireMiner` node spawns a dwarf; GameInstance holds `CurrentRockActor`.
🏁 **Milestone:** hands-free mining. 🧪 **Boss:** hire two dwarves; both damage the rock independently. 🎁 **Reward:** Master Smith · +300 XP.
⭐ **Optional — Impact-synced hits** via an [Anim Notify](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine).

---

## QUEST 9 — The Front Door 🚪 · ~50–55 min
A Main Menu level (New Game, Quit) that the game boots into, plus an Esc pause menu.

🎮 **After:** the game opens on a title screen — New Game → mine; Esc pauses to return to the menu or quit.
🛠️ **Teaches:** [UMG menus](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · Open Level & Input Modes ([Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)) · Quit Game.
🔑 **Key nodes:** `Open Level (by Name) · Set Input Mode Game And UI / UI Only · Set Show Mouse Cursor · Set Game Paused · Quit Game · Project Settings → Maps & Modes (Game Default Map)`
**Steps:** MainMenu level + `WBP_MainMenu` (set as Game Default Map) → New Game: reset GameInstance → Open Level Mine → Quit: Quit Game → `WBP_PauseMenu` (Esc): Resume, Quit to Main Menu, Quit Game; Set Input Mode UI Only + Set Game Paused while open.
💡 **Notes & gotchas:** the GameInstance persists across Open Level, so **New Game must explicitly reset it** — otherwise last run's progress leaks into the new game.
🏁 **Milestone:** title → New Game → mine; Esc pauses. 🧪 **Boss:** start, pause, quit to menu, start again — second run is fresh. 🎁 **Reward:** Master Smith · +250 XP.
⭐ **Optional — Title art.**

---

## QUEST 10 — Memory Core 💾 · ~55–60 min
SaveGame + a working Continue + Save/Load in the pause menu; loading rebuilds the world.

🎮 **After:** quit to desktop, relaunch, Continue → coins, upgrades, depth, and dwarves are all exactly back.
🛠️ **Teaches:** [SaveGame system](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine).
🔑 **Key nodes:** `Blueprint Class → SaveGame · Create Save Game Object · Save Game to Slot · Does Save Game Exist · Load Game from Slot · Cast To (SaveGame) · Delete Save Game in Slot · Now (UTC)`
**Steps:** `SG_DeepDelve` (SaveVersion, Coins, Depth, NodeLevels, current rock + pickaxe, dwarf count, LastSavedTime) → `SaveGame()` (Save button + autosave) → `RebuildWorld()` (set tier + pickaxe, re-spawn dwarves, refresh UI) → `LoadGame()` → **Continue** (only if a save exists) + **New Game** deletes the save.
💡 **Notes & gotchas:** ⚠️ **Save versioning** — later quests add fields. Store a `SaveVersion` int now and default any missing values on load, or old saves crash/load garbage.
🏁 **Milestone:** full persistence including dwarves. 🧪 **Boss:** delete the save and confirm a clean first-run works. 🎁 **Reward:** Master Smith · +300 XP.
⭐ **Optional — Multiple save slots.**

---

## QUEST 11 — Ship a Demo 📦 · ~40–50 min
Package a standalone build and send it to a friend — Blueprint-only, no C++ toolchain yet.

🎮 **After:** a double-clickable Deep Delve demo a friend can play with no Unreal installed.
🛠️ **Teaches:** [Packaging & build configs](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project).
🔑 **Key nodes:** `Platforms → Windows → Package Project · Build Configuration: Development · Project Settings → Maps & Modes · Project Settings → Description (name/icon)`
**Steps:** confirm Game Default Map + name/icon → Package (Development config) → test the build outside the editor (new game, upgrade, save, quit, relaunch, Continue) → zip and share.
💡 **Notes & gotchas:** 🧪 use your friend's feedback to **tune the economy** (see Q6's balancing note) before piling on content in Q13–16.
🏁 **Milestone:** a shippable demo. 🧪 **Boss:** run the build on another folder/machine, full play/save/quit/continue cycle. 🎁 **Reward:** Master Smith · +250 XP · 🎉.
⭐ **Optional — Feedback loop:** 3 questions for your friend.

---

## QUEST 12 — Into the Code 🧬 · ~60 min
Refactor the `EffectType` switch into a polymorphic `UTechEffect` hierarchy, built and run from Rider.

🎮 **After:** identical gameplay, but each node runs its own C++ effect object; you can hit breakpoints.
🛠️ **Teaches:** [C++ Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) · [Reflection macros](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [GameInstanceSubsystem](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine).
🔑 **Key nodes:** `Tools → New C++ Class · UCLASS(EditInlineNew,Blueprintable) · UPROPERTY(Instanced) · virtual Apply() · UGameInstanceSubsystem · GetGameInstanceSubsystem · Rider: build + attach debugger + breakpoints`
**Steps:** install **Rider** (Windows: MSVC build tools; macOS: Xcode CLT); Editor Preferences → Source Code Editor = Rider → base `UTechEffect` (EditInlineNew + Instanced) with virtual `Apply()` → subclasses (UpgradePickaxe, EquipPickaxe, Descend, HireMiner) → `DA_TechNode.Effect` becomes an instanced `UTechEffect*` → `Buy(node)` → `Effect->Apply(context)`; optionally a `UGameInstanceSubsystem` → **build & run from Rider with the debugger (no Live Coding; close the editor before a full rebuild).**
💡 **Notes & gotchas:** debug C++ in **Rider** — breakpoints + live variable inspection beat Print String for logic bugs.
🏁 **Milestone:** effects run in C++; breakpoints hit. 🧪 **Boss:** add a `UTechEffect_CoinMultiplier` subclass + node with zero edits to existing effects. 🎁 **Reward:** Deeplord · +300 XP.

---

## QUEST 13 — Sharper & Faster ⚡ · ~55 min
Pickaxe upgrades in the tech tree: damage, auto-swing (speed-upgradable), and critical hits (with their own sound).

🎮 **After:** your pickaxe swings on its own and occasionally lands a booming, gold "CRIT!".
🛠️ **Teaches:** new C++ [`UTechEffect` subclasses](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · random rolls · [UMG animation + crit sound](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine).
🔑 **Key nodes:** `Set Timer by Function Name (auto-swing) · Random Float in Range · Branch · WidgetAnimation (crit popup) · Play Sound 2D (crit)`
**Steps:** stats `bAutoSwing`, `AutoSwingInterval`, `CritChance`, `CritMultiplier` → auto-swing looping Timer (Auto-Swing unlock → Swing Speed) → crit roll per swing → gold popup + **🔊 crit sound** (Critical Strike → Critical Chance) → extend Sharpen → author nodes.
🏁 **Milestone:** auto-swing + flashy crits. 🧪 **Boss:** set CritChance 1.0 and drop the auto-swing interval. 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Crit feel:** camera shake + crits multiply coins.

---

## QUEST 14 — The Growing Crew 👥 · ~50–55 min
Buy a bigger crew and upgrade every dwarf's damage/speed — applied live to dwarves already in the mine.

🎮 **After:** the mine fills with dwarves and the whole crew speeds up at once when you upgrade.
🛠️ **Teaches:** global modifiers across live actors · [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [Data Assets (dwarf types)](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine).
🔑 **Key nodes:** `float multiplier variables · Event Dispatcher (broadcast) · Bind Event to Dispatcher · Clear and Invalidate Timer + Set Timer (reset) · Get All Actors Of Class`
**Steps:** `CrewDamageMult` / `CrewSpeedMult` (dwarf damage = `MinerData.Damage × mult`, interval = `Speed ÷ mult`) → subclasses `UpgradeCrewDamage`, `UpgradeCrewSpeed` → on change broadcast `OnCrewStatsChanged`; dwarves re-read + **reset their timer** → ring/line placement → optional new dwarf types.
🏁 **Milestone:** live crew upgrades. 🧪 **Boss:** buy Crew Speed twice with several dwarves mining; all speed up. 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Crew cap & formation.**

---

## QUEST 15 — Rich Veins 💎 · ~55 min
Vein yield upgrade, new vein tiers to descend into, and a lucky mineral (gem) drop.

🎮 **After:** richer tiers as you descend copper → gold → gems, multiplied payouts, and occasional jackpot gems.
🛠️ **Teaches:** content via [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) + reused Descend effects · random-chance drops · pickup reuse.
🔑 **Key nodes:** `new Data Asset instances · Random Float in Range · SpawnActor (BP_Mineral) · shared Collect() function · float multiplier math`
**Steps:** vein yield — global `CoinMult` (coins = `CoinReward × CoinMult`), node `UpgradeYield` → new veins — more `DA_RockData` tiers + Descend nodes → lucky mineral — `MineralChance`/`MineralValue`; on break roll, spawn `BP_Mineral` (collected by hover), nodes `UpgradeMineralChance`.
💡 **Notes & gotchas:** ⚖️ deeper tiers should raise `CoinReward` **faster** than `MaxHP`, or descending feels like a punishment. Keep `MineralChance` low — it's jackpot variance, not steady income.
🏁 **Milestone:** richer tiers + jackpot gems. 🧪 **Boss:** set MineralChance 1.0, descend a couple tiers, confirm deeper pays more. 🎁 **Reward:** Deeplord · +350 XP.
⭐ **Optional — Jackpot feel.**

---

## QUEST 16 — The Coin Collector 🧺 · ~50–55 min
A dwarf that walks right-to-left and auto-collects coins/minerals it touches. Speed-upgradable; buy more.

🎮 **After:** a dwarf hoovers up coins on its own — buy more and upgrade their speed to keep the floor clear.
🛠️ **Teaches:** [Collision & overlap events](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) · actor movement (Tick + delta) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine).
🔑 **Key nodes:** `Add Sphere/Box Collision · Generate Overlap Events (true) · OnComponentBeginOverlap · Event Tick · Delta Seconds · AddActorWorldOffset / SetActorLocation · shared Collect()`
**Steps:** `DA_CollectorData` + `BP_CollectorDwarf` (walk anim + overlap shape) → Tick: move left by `Speed × DeltaTime`, reset at the left edge → `OnComponentBeginOverlap` with coin/mineral → `Collect()` (shared function; reuses coin sound) → nodes Hire Collector + Collector Speed.
💡 **Notes & gotchas:** overlap needs **Generate Overlap Events = true on BOTH** the collector's shape and the coin's collision, with responses set to overlap — the usual reason overlaps silently don't fire.
🏁 **Milestone:** auto-sweeping collector. 🧪 **Boss:** pile coins, confirm it sweeps them, buy a second collector. 🎁 **Reward:** Deeplord · +300 XP.
⭐ **Optional — Collector polish.**

---

## QUEST 17 — Strike It Rich 🚀 · ~60 min · *finale*
Juice (animation, VFX, ambience) + a polished v1.0 Shipping build. Core SFX are already in.

🎮 **After:** a polished standalone v1.0 — clicks punch, coins ching, crits boom, dust and sparks fly.
🛠️ **Teaches:** [UMG Animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Audio & Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging a Shipping build](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project).
🔑 **Key nodes:** `Widget Animation (Designer track) · Sound Cue (create) · Spawn System at Location (Niagara) · Package Project (Shipping) · Project Settings → Description / Platforms icon`
**Steps:** UMG animations (DIG punch, purchase pops, unlock flourish) → audio: ambient/music, optionally upgrade to Sound Cues → Niagara: dust/sparks/shimmer/poof (+ optional camera shake) → ship v1.0: icon/splash/name, Shipping config, Package, test, share.
💡 **Notes & gotchas:** after adding C++ (Q12), packaging needs the compiler toolchain — but the packaged build is still self-contained; players need nothing installed.
🏁 **Milestone:** a polished, playable v1.0. 🧪 **Boss:** the Shipping build plays the whole game with sound + effects. 🎁 **Reward:** Deeplord, Shaper of the Deep · +400 XP · 🏅 QUEST LINE COMPLETE.
⭐ **Optional — Options & publish.**

---

## 🎓 Further learning
- [Epic Developer Community — Learning](https://dev.epicgames.com/community/unreal-engine/learning) — 20+ free official courses.
- [Your First Hour in Unreal Engine](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2) — the best gentle on-ramp.
- [Introduction to Unreal Engine](https://dev.epicgames.com/community/learning/courses/35z/introduction-to-unreal-engine) — foundations.
- [Tom Looman](https://tomlooman.com/) — deep Unreal **C++** tutorials (great for Q12+).
- [Epic Developer Community](https://dev.epicgames.com/community) & [Unreal Engine Forums](https://forums.unrealengine.com/) — where to ask when stuck.

**What to learn next:** Gameplay Ability System (GAS), replication & multiplayer, and AI / Behavior Trees — none needed for Deep Delve, but great directions for game #2.

---

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Docs: dev.epicgames.com*
