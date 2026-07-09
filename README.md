# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> ▶️ **Play the live interactive quest tracker:** https://fdvoracek.github.io/deep-delve/
>
> _Check off quests, watch your XP bar and Tech Map fill up. The full quest-by-quest guide — steps, milestones, rewards and documentation links — lives on the page and in [`docs/DeepDelve_QuestLog.md`](docs/DeepDelve_QuestLog.md)._

---

## 📜 About this quest line

You build **one complete game** across a series of bite-sized quests — each a single 30–60 minute sitting that ends with a visible change on screen. **Deep Delve** is an idle/incremental miner set in a Synty dwarven mine: you **click a rock face** to chip its HP, and when it breaks it **bursts into coins** you sweep up with the mouse. Spend those coins in a **tech tree** to sharpen your pickaxe, buy better picks, **hire animated dwarves** who mine alongside you, and **descend** to tougher, richer rock. The goal is depth — how far down can you dig?

You start in **Blueprints** (Unreal's visual scripting) for fast wins, then pivot to **C++** once the systems are real — the same journey a professional Unreal programmer takes. Architecture is first-class here: the player's damage lives on the Player Controller, rocks/pickaxes/miners are Data Assets, clicks travel through a Blueprint Interface, and every upgrade flows through one data-driven tech tree. You even **ship a playable demo** to a friend at Quest 11, well before the finish.

> **Status:** all 17 quests refined.

---

## 🧱 The technology stack

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
| Code | [C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Reflection](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) | Q12 |
| Collision | [Collision & Overlap events](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) | Q16 |
| Polish | [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) | Q17 |
| Art assets | [Synty POLYGON Dungeon Realms](https://syntystore.com/products/polygon-dungeon-realms) | All |

---

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Goal: go as deep as you can.*

**→ [Open the quest tracker](https://fdvoracek.github.io/deep-delve/)**
