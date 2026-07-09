# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> A gamified path from zero Unreal experience to a finished, shippable game.
> **Project:** an idle / incremental mining game called **Deep Delve**.
> **Engine:** Unreal Engine 5.8 (anything 5.6+ works identically).
> **Path:** Blueprints first, then C++. Built for a programmer who is new to Unreal.
> **Art:** Synty **POLYGON Dungeon Realms** (dwarf miners, forge machinery, modular mine tiles, crystals & gold).

---

## 📜 About this quest line

**What you're building.** One complete game, across 11 bite-sized quests. Each quest is a single 30–60 minute sitting that ends with a visible change on screen and a reward. **Deep Delve** is an idle/incremental miner: you swing a pickaxe to mine raw ore, hire dwarf miners and machines to auto-dig, smelt ore into **ingots** at the forge, and craft better pickaxes and tools that break into deeper, tougher rock. Deeper layers hold richer ore — copper → iron → silver → gold → mithril → raw gems.

**The goal is Depth.** How deep can you delve? Depth is your headline progress meter, and it shows on screen: as you dig, the camera sinks down the shaft through the Synty modular tiles. When progress slows, **collapse the shaft** (prestige) to earn permanent **Dwarven Renown** and delve again, faster and deeper. Three pillars, one loop: **Mine → Process → Craft → dig deeper → repeat.**

**How you'll build it.** You begin in **Blueprints** (Unreal's node-based visual scripting) for fast wins and motivation, then pivot to **C++** once the systems are real — the same journey a professional Unreal programmer takes. Every quest lists exactly which Unreal technology, tool or principle it teaches, and links to the official documentation.

### The technology stack

| Area | Technology / tool in Unreal | Learn it in |
|---|---|---|
| The engine & editor | [Unreal Engine 5.8 Editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) | Quest 1 |
| Importing art | [Fab / Content Browser importing](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine) | Quest 1 |
| User interface | [UMG (Unreal Motion Graphics)](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + [Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) | Quests 2, 6 |
| Game logic (visual) | [Blueprints Visual Scripting](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) | Quests 3, 4 |
| Game-wide state | [GameInstance & Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Quest 5 |
| Scheduling | [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) | Quest 5 |
| Data-driven content | [Structs](https://dev.epicgames.com/documentation/en-us/unreal-engine/structs-in-unreal-engine) + [Data Tables](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-driven-gameplay-elements-in-unreal-engine) | Quests 6, 7 |
| Persistence | [SaveGame system](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) | Quest 8 |
| Code | [Programming with C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) + [Live Coding](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime) | Quests 9, 10 |
| Polish | [UMG Animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) | Quest 11 |
| Shipping | [Packaging your project](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) | Quest 11 |
| Art assets | [Synty POLYGON Dungeon Realms](https://syntystore.com/products/polygon-dungeon-realms) | All |

---

## 🧭 Tech Map — everything you'll learn, and when

Mark each row done as you finish the quest that teaches it. This is your "learned" checklist — by the bottom you'll have touched every core system needed to ship an Unreal game.

| ✔ | Technology / tool / principle | What it implements in Deep Delve | Quest | Docs |
|---|---|---|---|---|
| ☐ | Unreal Editor & Projects | The tool you build in: viewport, Content Browser, PIE | 1 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) |
| ☐ | Fab / Importing assets | Getting the Synty pack into the project | 1 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine) |
| ☐ | UMG (Unreal Motion Graphics) | All on-screen UI: ore counter, DIG button, panels | 2 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) |
| ☐ | Widget Blueprints | Where each UI screen is designed | 2 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) |
| ☐ | Gameplay Framework (GameMode/PlayerController) | Input mode, cursor, active camera, HUD spawn | 2 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) |
| ☐ | Blueprints Visual Scripting | Node-based game logic | 3 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) |
| ☐ | Variables & Events | Ore state + OnClicked reaction | 3 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine) |
| ☐ | Blueprint Functions (pure & impure) | AddOre / FormatNumber | 4 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/overview-of-blueprints-visual-scripting-in-unreal-engine) |
| ☐ | Data types & Text formatting | Big-number floats, "1.5M" display | 4 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) |
| ☐ | GameInstance | Game-wide economy state | 5 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) |
| ☐ | Gameplay Timers | Ore-per-second production | 5 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) |
| ☐ | Structs | S_Miner / S_Tool data shapes | 6 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/structs-in-unreal-engine) |
| ☐ | Data Tables & data-driven design | Miners and depth tiers as editable data | 6 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-driven-gameplay-elements-in-unreal-engine) |
| ☐ | Dynamic UMG (Create Widget + Scroll Box) | Spawning a UI row per miner | 6 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) |
| ☐ | Exponential cost curves & tiered gating | Cost = base·growth^owned; depth unlocks | 6 | [link](https://en.wikipedia.org/wiki/Incremental_game) |
| ☐ | Enumerations & Booleans | Tool effect types; one-time "Crafted" | 7 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) |
| ☐ | Resource conversion & multipliers | Ore→ingots + stacking bonuses | 7 | [link](https://en.wikipedia.org/wiki/Incremental_game) |
| ☐ | SaveGame system | Persist all progress to disk | 8 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) |
| ☐ | Programming with C++ (Quick Start) | Your first Unreal C++ class | 9 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) |
| ☐ | Reflection macros (UCLASS/UPROPERTY/UFUNCTION) | Expose C++ to Blueprint/editor | 9 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) |
| ☐ | Subsystems (GameInstanceSubsystem) | Home for the C++ economy | 9 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) |
| ☐ | Live Coding | Recompile C++ without restarting | 9 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime) |
| ☐ | Date & Time (FDateTime/FTimespan) | Offline-earnings time math | 10 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Misc/FDateTime) |
| ☐ | Prestige / meta-progression | Cave-In for permanent Renown | 10 | [link](https://en.wikipedia.org/wiki/Incremental_game) |
| ☐ | UMG Animations | Click juice, floating "+N" | 11 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) |
| ☐ | Audio & Sound Cues | Pick-strike, forge clang | 11 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) |
| ☐ | Niagara VFX | Rock-dust & spark bursts | 11 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) |
| ☐ | Packaging & Shipping | Build a standalone .exe | 11 | [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) |

*(The interactive HTML tracker lights these up automatically as you complete quests — this table is for offline reference.)*

---

## How this works

You build **one game** across 11 small quests. Each quest is a **single sitting of 30–60 minutes**, ends with a **visible change in the game**, and hands you a **reward** plus a **Knowledge Card**. Later quests depend on earlier ones — no skipping foundations.

### The loop for every quest
1. **Brief** — what you're building and why it matters.
2. 🛠️ **Teaches** — the exact Unreal tech/tools/principles, each with a documentation link.
3. **Steps** — the actual work, kept short.
4. 🏁 **Milestone** — the visible proof it worked.
5. 🧪 **Boss check** — a tiny self-test to prove you understand it.
6. 🎁 **Reward** — a title, XP, and a suggested real-world treat.
7. 🗂️ **Knowledge Card** — the terms now in your toolkit.

### Rank progression
| Total XP | Rank |
|---|---|
| 0 | Prospector |
| 100 | Pit Digger |
| 350 | Miner |
| 750 | Pit Foreman |
| 1,300 | Master Smith |
| 2,000 | Deeplord |

---

## 🎒 The Asset Kit — Synty POLYGON Dungeon Realms

Imported in Quest 1. **1,100+ low-poly 3D prefabs** (Unity Awards 2021 winner), available in a native **Unreal Engine** version on the [Synty store](https://syntystore.com/products/polygon-dungeon-realms). Key pieces for Deep Delve: dwarf **miner** characters (with pickaxes), dwarven **forge machinery**, modular **walls/tiles/stairs/pillars** for the shaft, and **crystals/gems/gold coins** for ore and currency visuals.

**Important:** this is a **3D model pack, not a 2D UI kit**. Deep Delve uses it two ways: (1) a 3D mine diorama the camera frames behind your UI, and (2) icons captured from the meshes. Until you capture icons, plain text buttons work fine — art is polish, not a blocker.

---

## QUEST 1 — First Light 🌱
**Time:** 40–55 min · **Rank target:** Prospector

**Brief.** Get Unreal installed, create your project, and import the Synty pack so the dwarves are ready to work. Base camp before the descent.

🛠️ **Teaches:**
- **[Unreal Editor & Projects](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation)** — the tool you build everything in: Viewport, Content Browser, and Play-In-Editor (PIE).
- **[Fab / Importing assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine)** — how marketplace packs like Synty get into your project ([direct import reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/importing-assets-directly-into-unreal-engine)).

**Steps.**
1. Install the **Epic Games Launcher**, then **Unreal Engine 5.8** from the *Library* tab.
2. New project → **Games → Blank**. Blueprint, Desktop, **Starter Content OFF**. Name it `DeepDelve`.
3. Get **POLYGON Dungeon Realms** (Unreal version) from your Synty account; add via the **Fab** window / *Add to Project*, or copy the pack's `Content` into `Content/Synty`.
4. Open the pack's **demo scene** to confirm meshes/materials loaded; fly around with right-drag + WASD.
5. Make your folders in `Content/`: `UI`, `Blueprints`, `Data`, `Core` (keep Synty content separate).

🏁 **Milestone:** The project opens, the Synty demo scene renders dwarves and dungeon tiles, and your four folders exist.

🧪 **Boss check:** Search the Content Drawer and find the dwarf **miner** character and a **forge machinery** prop by name.

🎁 **Reward:** Title **Prospector** · +100 XP · ☕ Coffee.

🗂️ **Knowledge Card:** *Launcher · Blueprint vs C++ project · importing asset packs · Content Drawer search · PIE · Outliner.*

---

## QUEST 2 — The Mine Awakens 🕯️
**Time:** 50–60 min · **Rank target:** Prospector

**Brief.** An idle game is mostly UI — but Deep Delve has a living backdrop. Build the HUD (ore counter + DIG button) and drop a small 3D mine scene behind it.

🛠️ **Teaches:**
- **[UMG (Unreal Motion Graphics)](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine)** — Unreal's UI system; the tech that implements every on-screen element ([quick start](https://dev.epicgames.com/documentation/en-us/unreal-engine/umg-ui-designer-quick-start-guide-in-unreal-engine)).
- **[Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine)** — the asset type where each UI screen (WBP_Main) is designed and scripted.
- **[Gameplay Framework (GameMode / PlayerController)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)** — owns input mode, cursor, and camera; spawns your HUD at BeginPlay.

**Steps.**
1. Build a mine mouth from Synty **modular tiles**, add torches and a **dwarf miner**. Place a **Camera Actor** framing it.
2. In `Content/UI` create `WBP_Main`. Add `Txt_Ore` (value `0`, big) and a Button with a `⛏ DIG` text child (`Btn_Dig`).
3. Level Blueprint → **BeginPlay** → **Create Widget** (`WBP_Main`) → **Add to Viewport**.
4. **Set Input Mode Game And UI**, **Show Mouse Cursor**, **Set View Target with Blend** to your camera.
5. Play.

🏁 **Milestone:** A big `0` and a **DIG** button sit over a real 3D dwarven mine (it doesn't dig yet).

🧪 **Boss check:** Move the camera and confirm the HUD stays put while the 3D scene changes — UI-space vs world-space.

🎁 **Reward:** Title stays **Prospector** · +150 XP · 🍪 Snack break.

🗂️ **Knowledge Card:** *UMG · Widget Blueprint · Canvas Panel · Add to Viewport · Camera Actor · Set View Target · Input Mode.*

---

## QUEST 3 — Swing the Pickaxe ⛏️
**Time:** 30–45 min · **Rank target:** Miner (soon)

**Brief.** The core verb: click the pickaxe, mine ore, number goes up. Your first event-driven Blueprint logic.

🛠️ **Teaches:**
- **[Blueprints Visual Scripting](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — Unreal's node-based visual programming language.
- **[Variables & Events](https://dev.epicgames.com/documentation/en-us/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine)** — store state (the `Ore` variable) and react to things happening (the button's `OnClicked` event).

**Steps.**
1. In `WBP_Main` add an **Integer** `Ore`. Compile.
2. Select `Btn_Dig` → Details → **Events → + On Clicked**.
3. On the event: **Increment** `Ore` (+1 per swing).
4. Drag `Txt_Ore` in → **Set Text (Text)**, feed it `Ore`. Wire after the increment.
5. Play and dig.

🏁 **Milestone:** Every click, ore goes up. The heartbeat of the game exists.

🧪 **Boss check:** Add a "power swing" button that adds 5, sharing the same `Ore` variable.

🎁 **Reward:** Title **Pit Digger** · +150 XP · 🎵 One song.

🗂️ **Knowledge Card:** *Variables (Int) · Compile · OnClicked · Set/Increment · Set Text · implicit conversion nodes.*

---

## QUEST 4 — Rich Veins 🔢
**Time:** 45–60 min · **Rank target:** Miner

**Brief.** Idle games run on gorgeous huge numbers. Refactor into reusable functions and write an ore-formatting function.

🛠️ **Teaches:**
- **[Blueprint Functions (pure & impure)](https://dev.epicgames.com/documentation/en-us/unreal-engine/overview-of-blueprints-visual-scripting-in-unreal-engine)** — `AddOre` changes state; `FormatNumber` is a *pure* function that just returns a value.
- **[Data types & Text formatting](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — Float vs Int for big idle numbers, and Format Text to display `1.5M`.

**Steps.**
1. Change `Ore` to **Float**.
2. Create `AddOre(Amount: Float)` that adds to `Ore` and refreshes the UI. Route both dig buttons through it.
3. Create a **pure** `FormatNumber(Value) → Text`: `≥1e9` `"B"`, `≥1e6` `"M"`, `≥1e3` `"K"`, else raw; round to 1 decimal.
4. Create `RefreshUI` that sets `Txt_Ore` to `FormatNumber(Ore)`; call it from `AddOre`.

🏁 **Milestone:** Past 1,000 shows `1.0K`; past a million shows `1.0M`.

🧪 **Boss check:** Feed `FormatNumber` `1500000` and confirm it prints `1.5M`.

🎁 **Reward:** +250 XP (pushes you toward **Miner** rank) · 🚶 5-minute walk.

🗂️ **Knowledge Card:** *Functions vs pure functions · Float · Branch · Format Text · rounding · single source of truth.*

---

## QUEST 5 — Hire the Crew ⚙️
**Time:** 45–60 min · **Rank target:** Miner

**Brief.** The "idle": dwarves that dig while you don't. Add ore-per-second and move core numbers into a GameInstance that outlives any screen.

🛠️ **Teaches:**
- **[GameInstance (Gameplay Framework)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine)** — a single object holding game-wide state that survives level loads; the home for your economy.
- **[Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine)** — run `Produce()` once per second instead of every frame; how idle income ticks.

**Steps.**
1. Create **Blueprint Class → GameInstance** → `BP_GameInstance`. Set it in *Project Settings → Maps & Modes*.
2. Move `Ore` and add `OrePerSecond` (Float). Widget reads via **Get Game Instance → Cast**.
3. In **Init**, **Set Timer by Function Name** → `Produce`, `1.0`s, looping. `Produce` calls `AddOre(OrePerSecond)`.
4. Set `OrePerSecond = 1`; place dwarf miner meshes in the scene so the income has a face.

🏁 **Milestone:** Start the game, touch nothing — ore climbs on its own while the dwarves swing.

🧪 **Boss check:** Explain why a 1s Timer beats per-frame Tick here (fixed cadence, no delta math).

🎁 **Reward:** Title **Miner** · +250 XP · 🎮 Play someone else's idle game 10 min.

🗂️ **Knowledge Card:** *GameInstance · Cast To · Set Timer by Function Name · looping timers · Event Tick · delta time.*

---

## QUEST 6 — The Dig Deepens 🪨
**Time:** ~60 min (splittable) · **Rank target:** Pit Foreman

**Brief.** The signature quest. Add buyable **miners/machines** (data-driven, rising costs) AND introduce **Depth** — the goal. As you dig, the camera descends the shaft through Synty tiles and deeper layers reveal richer ore.

🛠️ **Teaches:**
- **[Structs](https://dev.epicgames.com/documentation/en-us/unreal-engine/structs-in-unreal-engine)** — a custom data shape (`S_Miner`) grouping name, cost, production, owned count.
- **[Data Tables & data-driven design](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-driven-gameplay-elements-in-unreal-engine)** — store all miners and depth tiers as editable data rows; adding content = adding data, not code.
- **[Dynamic UMG (Create Widget + Scroll Box)](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine)** — spawn a UI row per miner at runtime and list them in a scrollable panel.
- **[Exponential cost curves & tiered gating](https://en.wikipedia.org/wiki/Incremental_game)** *(design principle)* — cost = base·growth^owned; content unlocked by Depth thresholds.

**Steps.**
1. Struct `S_Miner`: `DisplayName`, `BaseCost`, `CostGrowth (~1.15)`, `BaseProduction`, `Owned`.
2. **Array** of `S_Miner` with 3 entries (Dwarf Digger, Ore Cart, Drill Machine — use pack meshes).
3. Cost (pure fn): `CurrentCost = BaseCost * (CostGrowth ^ Owned)`.
4. Reusable `WBP_MinerRow` with `Setup(index)`; in `WBP_Main` loop the array into a **Scroll Box** (Create Widget → Setup → Add Child).
5. HIRE: if `Ore ≥ cost` → subtract, `Owned++`, recompute `OrePerSecond`, refresh.
6. **Depth:** float `Depth = LifetimeOre / 500`. Bind the mine camera's Z to `Depth` so it sinks. Add `Txt_Depth` + ore tiers (Copper 0m, Iron 100m, Silver 300m, Gold 700m, Mithril 1500m).

🏁 **Milestone:** Hire miners, ore/sec climbs, and the camera visibly sinks deeper with a live depth readout and changing ore tier.

🧪 **Boss check:** Add a 4th miner by adding **one data row** and no new nodes. If it just works, your design is right.

🎁 **Reward:** Title **Pit Foreman** · +300 XP · 🍕 A proper meal.

🗂️ **Knowledge Card:** *Struct · Array · Data Table · exponential cost scaling · dynamic widgets · Scroll Box · driving world state from a variable · tiered gating.*

---

## QUEST 7 — The Forge & the Anvil 🔥
**Time:** 50–60 min · **Rank target:** Pit Foreman

**Brief.** The other two pillars: smelt raw ore into **ingots** at the forge (resource conversion), then **craft** better pickaxes/tools that multiply dig power and unlock deeper rock.

🛠️ **Teaches:**
- **[Enumerations & Booleans](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine)** — an enum names the tool's effect type (SwingPower / GlobalYield / DepthUnlock); a Bool tracks one-time `Crafted` state.
- **[Resource conversion & multipliers](https://en.wikipedia.org/wiki/Incremental_game)** *(design principle)* — a second economy loop (ore→ingots) plus stacking multipliers.

**Steps.**
1. Add `Ingots` (Float) and a **Smelt** action: convert `N` ore into `1` ingot (Synty forge machinery as the smelter).
2. Struct `S_Tool`: `Name`, `IngotCost`, `Type (enum)`, `Value`, `Crafted (Bool)`. Add a few.
3. Add `SwingMultiplier`, `YieldMultiplier` (default 1), `MaxDepthAllowed`. Apply to swing / per-second / Depth cap.
4. **Craft**: if enough ingots & not crafted → spend, set `Crafted`, apply effect (multiplier or raise `MaxDepthAllowed`), disable button.
5. Tint/enable craft buttons by affordability.

🏁 **Milestone:** Smelt ore into ingots, craft an Iron Pickaxe that boosts digging, and a tool that unlocks a deeper layer.

🧪 **Boss check:** Confirm a tool can't be crafted twice, and that hitting the depth cap stops you until you craft the unlock.

🎁 **Reward:** Title stays **Pit Foreman** · +300 XP · 🎧 Music + stretch.

🗂️ **Knowledge Card:** *Enums · Booleans / one-time crafts · resource conversion · multiplier stacking · Depth gating · SetIsEnabled.*

---

## QUEST 8 — Memory Core 💾
**Time:** 45–60 min · **Rank target:** Pit Foreman

**Brief.** A miner who loses progress on quit is a sad miner. Persist everything so the shaft survives closing the game.

🛠️ **Teaches:**
- **[SaveGame system](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine)** — Unreal's built-in persistence: a `USaveGame` object plus **Save/Load Game to Slot** writes progress to disk.

**Steps.**
1. **Blueprint Class → SaveGame** → `SG_DeepDelve`. Mirror: `Ore`, `Ingots`, `Depth`, `LifetimeOre`, miner array, tool array, `MaxDepthAllowed`, `LastSavedTime`.
2. `SaveGame`: **Create Save Game Object** → copy state → `LastSavedTime = UtcNow` → **Save Game to Slot** (`"DeepDelve"`, 0).
3. `LoadGame`: **Does Save Game Exist** → **Load** → **Cast** → copy back → recompute per-second.
4. Call `LoadGame` on Init; `SaveGame` on a 10s timer + a manual **Save** button.

🏁 **Milestone:** Hire miners, craft a tool, fully close the game, reopen — everything's still there at the same depth.

🧪 **Boss check:** Delete the save (or use a fresh slot) and confirm a clean first-run still works.

🎁 **Reward:** Title **Keeper of the Deep** · +300 XP · 😴 Guilt-free early stop.

🗂️ **Knowledge Card:** *USaveGame · Save/Load Game to/from Slot · Does Save Game Exist · serialization · UTC timestamp.*

---

## QUEST 9 — Into the Code 🧬
**Time:** ~60 min · **Rank target:** Master Smith

**Brief.** The pivot to C++. Add C++ to the project and move the mining/processing/depth math into a C++ subsystem, called from your Blueprints. Blueprint = view, C++ = model.

🛠️ **Teaches:**
- **[Programming with C++ (Quick Start)](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start)** — set up the toolchain and write your first Unreal C++ class ([C++ overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine)).
- **[Reflection macros: UCLASS / UPROPERTY / UFUNCTION](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine)** — expose C++ classes, variables and functions to Blueprint/editor (e.g. `BlueprintCallable`).
- **[Subsystems (GameInstanceSubsystem)](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine)** — an auto-instanced, managed home for your C++ economy.
- **[Live Coding](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime)** — recompile C++ (Ctrl+Alt+F11) while the editor keeps running.

**Steps.**
1. Install **Visual Studio 2022** (*Game development with C++*) or **Xcode** on macOS. **Tools → New C++ Class**.
2. Create a **GameInstanceSubsystem** → `UMineSubsystem`.
3. Expose doubles `Ore/Ingots/Depth/OrePerSecond` (`BlueprintReadOnly`) and `BlueprintCallable` `Dig`, `TryHireMiner`, `TryCraftTool`, `Smelt` + `BlueprintPure` `FormatNumber`.
4. Implement the math in `.cpp`; Compile via Live Coding (Ctrl+Alt+F11).
5. In BP: **Get Game Instance Subsystem (MineSubsystem)** → call the C++ functions; delete duplicated BP math.

🏁 **Milestone:** The game plays identically, but digging, hiring, smelting and depth now run through C++. Set a breakpoint and watch your own game hit it.

🧪 **Boss check:** Change a C++ formula, Live Coding, see the running game update without a full restart.

🎁 **Reward:** Title **Master Smith** · +350 XP · 🏆 You bridged Blueprint and C++.

🗂️ **Knowledge Card:** *C++ setup · UCLASS/UPROPERTY/UFUNCTION · BlueprintCallable/Pure/ReadOnly · GameInstanceSubsystem · Live Coding · model/view split.*

---

## QUEST 10 — Cave-In ♻️
**Time:** ~60 min · **Rank target:** Master Smith

**Brief.** The two mechanics that make an idle game unstoppable: **prestige** (collapse the shaft, reset, keep a permanent bonus) and **offline earnings**. Both in C++, both use time math.

🛠️ **Teaches:**
- **[Date & Time (FDateTime / FTimespan)](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Misc/FDateTime)** — Unreal's time classes; compare the saved timestamp to now to compute offline earnings.
- **[Prestige / meta-progression](https://en.wikipedia.org/wiki/Incremental_game)** *(design principle)* — reset-for-permanent-bonus loops (Dwarven Renown) that sustain long-term play.

**Steps.**
1. **Offline:** on load read `LastSavedTime`, get `FDateTime::UtcNow()`, compute elapsed `FTimespan`. Grant `OrePerSecond * seconds`, **capped** (~8h). Fire a "while you slept" popup.
2. **Cave-In:** add `DwarvenRenown`. `CaveIn()` computes renown from `LifetimeOre` (`floor(sqrt(LifetimeOre/1e6))`), adds it, resets ore/ingots/miners/tools/depth.
3. Apply permanent `1 + Renown*0.02` multiplier to all yield.
4. Add a **COLLAPSE SHAFT** button (enabled past a depth threshold) + renown readout. Save `DwarvenRenown`.

🏁 **Milestone:** Close for a few minutes → reopen → offline ore. Collapse the shaft → everything resets but you're permanently stronger.

🧪 **Boss check:** After a cave-in, confirm `LifetimeOre`/Renown persisted while current ore reset to zero.

🎁 **Reward:** Title **Deeplord** · +400 XP · 🎉 A complete game loop.

🗂️ **Knowledge Card:** *FDateTime/FTimespan · UtcNow · offline cap · prestige / meta-progression · permanent multipliers · popups.*

---

## QUEST 11 — Strike It Rich 🚀
**Time:** ~60 min · **Rank target:** Deeplord (Victory Lap)

**Brief.** Make it *feel* good, then package it into something you can double-click and hand to a friend. "Done" means shipped.

🛠️ **Teaches:**
- **[UMG Animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine)** — keyframed widget animations (scale-punch, floating "+N") that make clicks satisfying.
- **[Audio & Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine)** — the node-based audio asset that plays your pick-strike and forge clang.
- **[Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine)** — Unreal's particle system for rock-dust and spark bursts.
- **[Packaging & Shipping](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project)** — cook and build your project into a standalone `.exe`.

**Steps.**
1. **Juice:** UMG scale-punch on DIG; a floating "+N ore" that drifts up and fades; play a dwarf mining animation on dig.
2. **Sound:** import a pick-strike and forge clang → **Sound Cues** → **Play Sound 2D** on dig/smelt.
3. *(Optional)* A small **Niagara** rock-dust/sparks burst at the strike.
4. **Package:** *Platforms → Windows → Package Project* (Shipping config). Run the produced `.exe` outside the editor.

🏁 **Milestone:** A standalone **Deep Delve** executable with sound, dust, and descending depth — playable by anyone.

🧪 **Boss check:** Does the packaged build dig, auto-mine, smelt, craft, save, go deeper, and cave-in with sound? If yes: **you made a game.**

🎁 **Reward:** Title **Deeplord, Shaper of the Deep** · +400 XP · 🏅 **QUEST LINE COMPLETE.**

🗂️ **Knowledge Card:** *UMG animations · Sound Cue · Play Sound 2D · Niagara basics · Shipping config · Package Project.*

---

## 🏆 Completion — what you'll know

Hands-on command of: the Unreal Editor and project workflow; importing and using a Synty asset pack; UMG UI (static and data-driven); Blueprint variables, functions, events, structs, arrays, enums, Data Tables; GameInstance and Subsystems; timers and delta time; driving world/camera state from data (the descending shaft); the SaveGame system; the full pivot into **C++** with reflection macros, `BlueprintCallable` APIs, and Live Coding; time-based systems (offline progress); meta-progression design (prestige); and packaging a shipping build. Plus a finished, playable, saveable dwarf-mining game to prove it.

## 🎯 Staying motivated (read this on hard days)
- **One quest per session. One session per day.** 30 minutes counts.
- **Never end mid-quest.** Always stop on a green milestone so tomorrow starts with a win.
- **Mark it done in the tracker.** Watching the depth, your XP bar, and the Tech Map light up is the point.
- **Stuck 20+ min?** Note the exact question, take the reward break, and bring me the specific blocker.
- **Art is polish, not a blocker.** Use text buttons and a grey box mine, keep coding, add the dwarves later.

---
*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Goal: go as deep as you can. Docs: dev.epicgames.com.*
