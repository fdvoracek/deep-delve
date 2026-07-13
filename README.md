# ⛏️ Deep Delve — Learn Unreal Engine by Building a Game

> ▶️ **Live quest line:** https://fdvoracek.github.io/deep-delve/ — work through the quests, checking each off as you build.

**Deep Delve** is an idle/incremental miner built in **Unreal Engine 5.8**: click a rock to chip its HP, break it for a burst of coins, spend them in a tech tree to sharpen and auto-swing your pickaxe, hire animated dwarves who mine and collect for you, and descend to tougher, richer rock. You build the whole thing across a chain of 30–60 minute quests, each ending in a visible win, and ship a playable demo partway through. Art: **Synty POLYGON Dungeon Realms**.

## The path: C++ first

Every system is a C++ class from Quest 1, with a thin **Blueprint subclass** that only assigns data (meshes, sounds, Data Assets, input assets) and sets `UPROPERTY` defaults. UMG uses C++ `UUserWidget` base classes with `meta=(BindWidget)` controls and `WBP_` layouts. Each quest lists the classes/methods it adds, the matching Blueprints, and a UPROPERTY→value table — so you learn the "real Unreal" architecture, not just a click handler.

The page is fully self-contained: the complete quest log is embedded and rendered in the browser, so it works offline and is the single source used during development.

## 🧰 Before you start

- A machine that runs **UE 5.8** (~100+ GB free disk) and a modern GPU.
- The **Synty POLYGON Dungeon Realms** pack — a zipped UE project you **migrate** into your game (Quest 1). You must own it; keep the assets out of a public repo.
- **JetBrains Rider** (free for non-commercial) + a compiler toolchain (Windows: MSVC build tools; macOS: Xcode command-line tools) — needed from Quest 1.
- A **GitHub account** for version control and hosting this tracker.

## Repo layout

```
docs/
├── index.html                    ← the interactive quest tracker (GitHub Pages serves this)
├── DeepDelve_QuestLog.md         ← the quest log (also embedded in index.html)
└── architecture/                 ← architecture diagrams (also embedded in index.html)
    ├── quest-1.svg … quest-16.svg   architecture after each quest (new pieces in gold)
    └── final.svg                    the complete class diagram
```

Each quest card in the tracker shows the architecture diagram for that quest inline, and the finished architecture appears at the top of the page — all embedded in `index.html`, so the page stays self-contained. The standalone `.svg` files in `docs/architecture/` are the same diagrams for viewing/linking on their own.

Hosting: GitHub Pages, **Deploy from a branch → main → `/docs`**.

Build helpers (run after editing content):

- `build_cpp_index.js` re-embeds `docs/DeepDelve_QuestLog.md` into `docs/index.html`, keeping the self-contained page in sync.
- `build_diagrams.py` regenerates all `docs/architecture-*.svg` files and re-injects them into `docs/index.html`. Run it, then run `build_cpp_index.js`.

## 🎓 Further learning

[Epic's free courses](https://dev.epicgames.com/community/unreal-engine/learning) · [Your First Hour in Unreal Engine](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2) · [Tom Looman (C++)](https://tomlooman.com/) · [Unreal Engine Forums](https://forums.unrealengine.com/). After this game: GAS, replication/multiplayer, and AI.
