# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> ▶️ **Play the live interactive quest tracker:** https://fdvoracek.github.io/deep-delve/
>
> _Check off quests, watch your XP bar and Tech Map fill up. The full quest-by-quest guide — steps, milestones, rewards and documentation links — lives on the page._

---

## 📜 About this quest line

**What you're building.** One complete game, across 11 bite-sized quests. Each quest is a single 30–60 minute sitting that ends with a visible change on screen and a reward. **Deep Delve** is an idle/incremental miner: you swing a pickaxe to mine raw ore, hire dwarf miners and machines to auto-dig, smelt ore into **ingots** at the forge, and craft better pickaxes and tools that break into deeper, tougher rock. Deeper layers hold richer ore — copper → iron → silver → gold → mithril → raw gems.

**The goal is Depth.** How deep can you delve? Depth is your headline progress meter, and it shows on screen: as you dig, the camera sinks down the shaft through the Synty modular tiles. When progress slows, **collapse the shaft** (prestige) to earn permanent **Dwarven Renown** and delve again, faster and deeper. Three pillars, one loop: **Mine → Process → Craft → dig deeper → repeat.**

**How you'll build it.** You begin in **Blueprints** (Unreal's node-based visual scripting) for fast wins and motivation, then pivot to **C++** once the systems are real — the same journey a professional Unreal programmer takes. Art comes from the **Synty POLYGON Dungeon Realms** pack (dwarf miners, forge machinery, modular mine tiles, gems & gold).

---

## 🧱 The technology stack

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

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Goal: go as deep as you can.*

**→ [Open the quest tracker](https://fdvoracek.github.io/deep-delve/)**
