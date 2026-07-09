# ⛏️ DEEP DELVE — The Unreal Engine Quest Log

> ▶️ **Play the live interactive quest tracker:** https://fdvoracek.github.io/deep-delve/
>
> _Check off quests, watch your XP bar and Tech Map fill up. The full quest-by-quest guide — steps, milestones, rewards and documentation links — lives on the page and in [`docs/DeepDelve_QuestLog.md`](docs/DeepDelve_QuestLog.md)._

---

## 📜 About this quest line

You build **one complete game** across a series of bite-sized quests — each a single 30–60 minute sitting that ends with a visible change on screen. **Deep Delve** is an idle/incremental miner set in a Synty dwarven mine: you **click a rock face** to chip its HP, and when it breaks it **bursts into coins** you sweep up with the mouse. Spend those coins in a **tech tree** to sharpen your pickaxe, buy better picks, **hire animated dwarves** who mine alongside you, and **descend** to tougher, richer rock. The goal is depth — how far down can you dig?

You start in **Blueprints** (Unreal's visual scripting) for fast wins, then pivot to **C++** once the systems are real — the same journey a professional Unreal programmer takes. Architecture is first-class here: the player's damage lives on the Player Controller, rocks and pickaxes are Data Assets, clicks travel through a Blueprint Interface, and every upgrade flows through one data-driven tech tree.

> **Status:** Quests 1–8 are fully refined; Quests 9–12 are drafts still being designed.

---

## 🧱 The technology stack

| Area | Technology / tool in Unreal | Learn it in |
|---|---|---|
| Engine & editor | [Unreal Engine 5.8 Editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-8-documentation) | Q1 |
| Importing art | [Fab window](https://dev.epicgames.com/documentation/en-us/unreal-engine/fab-window-in-unreal-engine) | Q1 |
| Interactive world objects | [Blueprint Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprints-visual-scripting-in-unreal-engine) | Q2 |
| Input | [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) | Q3 |
| Decoupled messaging | [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) | Q3, Q8 |
| Player state | [Player Controller / Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q3 |
| User interface | [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) + [Widget Components](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-components-in-unreal-engine) + [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) | Q3, Q4, Q7 |
| Economy state | [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) | Q4 |
| UI events | [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) | Q4 |
| Data-driven content | [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) | Q5, Q6, Q8 |
| Scheduling | [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) | Q8 |
| Animation | [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) + [Anim Notifies](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine) | Q8 |
| Persistence | [SaveGame](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) | Q9 |
| Code | [C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) + [Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) + [Live Coding](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime) | Q10 |
| Polish & shipping | [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) | Q11–Q12 |
| Art assets | [Synty POLYGON Dungeon Realms](https://syntystore.com/products/polygon-dungeon-realms) | All |

---

*Engine: Unreal Engine 5.8 · Art: Synty POLYGON Dungeon Realms · Path: Blueprints → C++ · Goal: go as deep as you can.*

**→ [Open the quest tracker](https://fdvoracek.github.io/deep-delve/)**
