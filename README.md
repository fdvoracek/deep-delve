# ⛏️ Deep Delve — Learn Unreal Engine by Building a Game

> ▶️ **Live hub:** https://fdvoracek.github.io/deep-delve/ — choose your path, then work through the quests.

**Deep Delve** is an idle/incremental miner built in **Unreal Engine 5.8**: click a rock to chip its HP, break it for a burst of coins, spend them in a tech tree to sharpen and auto-swing your pickaxe, hire animated dwarves who mine and collect for you, and descend to tougher, richer rock. You build the whole thing across a chain of 30–60 minute quests, each ending in a visible win, and ship a playable demo partway through. Art: **Synty POLYGON Dungeon Realms**.

## Two learning paths (same game)

| Path | For whom | Start |
|---|---|---|
| 🔵 **Blueprints-first** | Newer to Unreal, or want instant results — build in visual scripting, then pivot to C++ | [`/blueprints/`](https://fdvoracek.github.io/deep-delve/blueprints/) · [quest log](docs/blueprints/DeepDelve_QuestLog.md) |
| 🟢 **C++-first** | Experienced programmers who want the "real Unreal" architecture from Quest 1 | [`/cpp/`](https://fdvoracek.github.io/deep-delve/cpp/) · [quest log](docs/cpp/DeepDelve_CPP_QuestLog.md) |

The C++-first path builds every system as a C++ class with a thin Blueprint subclass that only assigns data (meshes, sounds, Data Assets) and `UPROPERTY` defaults; each quest lists the classes/methods, the matching Blueprints, and a UPROPERTY→value table.

## 🧰 Before you start (both paths)

- A machine that runs **UE 5.8** (~100+ GB free disk) and a modern GPU.
- The **Synty POLYGON Dungeon Realms** pack — a zipped UE project you **migrate** into your game (Quest 1). You must own it; keep the assets out of a public repo.
- **JetBrains Rider** (free for non-commercial) + a compiler toolchain (Windows: MSVC build tools; macOS: Xcode command-line tools) — needed from Quest 1 on the C++ path, Quest 12 on the Blueprints path.
- A **GitHub account** for version control and hosting these trackers.

## Repo layout

```
docs/
├── index.html               ← the hub (GitHub Pages serves this)
├── blueprints/
│   ├── index.html            ← Blueprints-first interactive tracker
│   └── DeepDelve_QuestLog.md
└── cpp/
    ├── index.html            ← C++-first path page
    └── DeepDelve_CPP_QuestLog.md
```

Hosting: GitHub Pages, **Deploy from a branch → main → `/docs`**.

## 🎓 Further learning

[Epic's free courses](https://dev.epicgames.com/community/unreal-engine/learning) · [Your First Hour in Unreal Engine](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2) · [Tom Looman (C++)](https://tomlooman.com/) · [Unreal Engine Forums](https://forums.unrealengine.com/). After this game: GAS, replication/multiplayer, and AI.
