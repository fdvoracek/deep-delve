# 🟢 DEEP DELVE — C++-First Quest Log

> The **C++-first** path: every system is a C++ class from Quest 1, with a thin **Blueprint subclass** that only assigns data (meshes, sounds, Data Assets, input assets) and sets `UPROPERTY` defaults. UMG uses C++ `UUserWidget` base classes with `meta=(BindWidget)` controls and `WBP_` layouts.
>
> **Engine:** Unreal Engine 5.8 · **IDE:** JetBrains Rider · **Art:** Synty POLYGON Dungeon Realms · **API macro used below:** `DEEPDELVE_API` (replace with your module's API macro).

## The working pattern (read once)

For every class we build:
1. **Write the C++ class** — `UPROPERTY` members expose data; specifiers decide who edits them (`EditDefaultsOnly` for class-default assets/tuning, `EditAnywhere` for per-instance, `VisibleAnywhere`/`BlueprintReadOnly` for runtime state, `BlueprintCallable`/`BlueprintNativeEvent` for BP access).
2. **Create a Blueprint subclass** (`BP_…` / `WBP_…`) — assign the values to those `UPROPERTY`s and drop the actor into the level.
3. **Data lives in `UPrimaryDataAsset`s** (`DA_…` instances) referenced by the classes.

Each quest lists: **📚 Teaches** (+ resources), **🎮 After this quest**, **💡 Why**, **🏛️ Patterns & principles**, **🔑 Key nodes / functions**, the **C++ classes & methods**, the matching **Blueprints**, a **UPROPERTY → value** table, **Steps**, and Milestone/Boss/Reward.

## 🏛️ Architecture & patterns you'll learn

This chain teaches *how to structure a game* as much as it teaches Unreal. Everything below is introduced in the quest noted and reused afterwards — so you finish able to name and apply each one.

**Clean-code & SOLID principles**
- **[Single Responsibility](https://en.wikipedia.org/wiki/Single-responsibility_principle)** — each class does one job: GameMode = rules, PlayerController = input/player, Subsystem = economy, Data Asset = data, Widget = view *(Q1+)*.
- **[Open/Closed](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)** — extend behaviour by adding classes, not editing existing ones *(Q6, the tech-effect hierarchy)*.
- **[Dependency Inversion](https://en.wikipedia.org/wiki/Dependency_inversion_principle)** — depend on an interface, not a concrete type *(Q3, `IMineable`)*.
- **[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)** — write an action once and reuse it *(Q4/Q12 shared swing + damage-number path)*.
- **Unreal C++ hygiene** — Epic naming (`A`/`U`/`F`/`I`/`E`/`b`), `override`/`final`, const-correctness, forward declarations, null-guards. See the **[Epic C++ Coding Standard](https://dev.epicgames.com/documentation/en-us/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine)**.
- **Memory safety** — every UObject/actor reference held in C++ is a **`UPROPERTY()`** (usually a `TObjectPtr`) so the garbage collector tracks it and never frees it out from under you. See **[Unreal Object Handling](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-object-handling-in-unreal-engine)** and **[Object Pointers](https://dev.epicgames.com/documentation/en-us/unreal-engine/object-pointers-in-unreal-engine)** *(introduced Q3)*.

**Design patterns**
- **[Interface](https://refactoring.guru/design-patterns) (Dependency Inversion)** — `IMineable` *(Q3)*.
- **[Observer / pub-sub](https://gameprogrammingpatterns.com/observer.html)** — dynamic multicast delegates: `OnCoinsChanged`, `OnDamageDealt` *(Q4)*, `OnTechChanged` *(Q6)*, `OnCrewStatsChanged` *(Q13)*. ([pattern](https://refactoring.guru/design-patterns/observer))
- **[Strategy](https://refactoring.guru/design-patterns/strategy)** — polymorphic `UTechEffect` with a virtual `Apply()` *(Q6)*.
- **[Service Locator](https://gameprogrammingpatterns.com/service-locator.html)** — the `UGameInstanceSubsystem` reached via `GetSubsystem<>()` *(Q4)*.
- **[Type Object](https://gameprogrammingpatterns.com/type-object.html) / data-driven** — Data Assets define content without new code *(Q5)*.
- **Model–View separation** — C++ `UUserWidget` base + `WBP_` layout via `BindWidget` *(Q4/Q7)*.
- **[Update Method](https://gameprogrammingpatterns.com/update-method.html) vs Timers** — per-frame `Tick` only when needed (the collector, Q15); scheduled **[Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine)** for cadenced work (dwarves Q8, auto-swing Q12).
- **[Object Pool](https://gameprogrammingpatterns.com/object-pool.html)** — reuse spawned actors instead of constant spawn/destroy churn *(noted in Q4, applied as the Q16 optimization; see [UE performance considerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-memory-and-cpu-performance-considerations-in-unreal-engine))*.

**Companion reading:** [Epic C++ Coding Standard](https://dev.epicgames.com/documentation/en-us/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine) · [Game Programming Patterns (free book)](https://gameprogrammingpatterns.com/) · [Refactoring Guru — Design Patterns](https://refactoring.guru/design-patterns).

## The C++-first arc

1. Foundations & First Class — 2. The Player & Enhanced Input — 3. The Rock & the Interface — 4. Coins & the Economy Subsystem — 5. Data-Driven Content — 6. Tech Tree: Model — 7. Tech Tree: UI — 8. Hire the Crew — 9. Menus & Level Flow — 10. Save & Load — 11. Ship a Demo — 12. Pickaxe Upgrades — 13. The Growing Crew — 14. Rich Veins — 15. The Coin Collector — 16. Strike It Rich.

> **Status:** all 16 quests fully specified, dependency-audited, and annotated with the architecture patterns they teach.

---

# QUEST 1 — Foundations & First Class · ~60 min

**Goal.** Stand up a **C++** project, wire up Rider, migrate the Synty assets, compose the mine scene, and create your first C++ class — the GameMode — with its Blueprint.

📚 **Teaches:** setting up a C++ Unreal project with Rider, migrating assets between projects, and writing your first class — a Game Mode — with a Blueprint subclass.
**Resources:** [Game Mode & Game State](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) · [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Gameplay Classes (C++/BP)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine) · [C++ Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) · [Epic C++ Coding Standard](https://dev.epicgames.com/documentation/en-us/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine) · *Further:* [Your First Hour in Unreal Engine](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2)

🎮 **After this quest:** the project compiles from Rider, your mine scene renders through a fixed camera, and your own `ADeepDelveGameMode` (via `BP_DeepDelveGameMode`) is the active game mode.

💡 **Why:** A [**Game Mode**](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) is the session's rulebook — it decides *which* PlayerController, Pawn, HUD and GameState classes are spawned. Even a click game needs one so the engine spawns *our* controller (Q2). We extend **`AGameModeBase`** (the lightweight base) rather than `AGameMode` because Deep Delve is single-player.

🏛️ **Patterns & principles:** **[Single Responsibility](https://en.wikipedia.org/wiki/Single-responsibility_principle)** — the GameMode owns *rules*, nothing else. The **C++-class + Blueprint-subclass split** ([Gameplay Classes](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine)) keeps behaviour in code and configuration in data. Follow the **[Epic C++ Coding Standard](https://dev.epicgames.com/documentation/en-us/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine)** from line one: `A`-prefixed actor classes, `override` on virtuals, one class per pair of `.h`/`.cpp` files.

🔑 **Key nodes / functions:** `Games→Blank→C++ template · Tools→New C++ Class (GameModeBase) · Editor Preferences→Source Code Editor=Rider · Asset Actions→Migrate · Add→Camera Actor · Camera→Auto Activate for Player 0 · UE_LOG · Project Settings→Maps & Modes`

### 🧩 C++ classes & methods
**Create `ADeepDelveGameMode : AGameModeBase`.**
```cpp
// DeepDelveGameMode.h
UCLASS()
class DEEPDELVE_API ADeepDelveGameMode : public AGameModeBase
{
    GENERATED_BODY()
public:
    ADeepDelveGameMode();
};
// .cpp
ADeepDelveGameMode::ADeepDelveGameMode()
{
    DefaultPawnClass = nullptr;  // click game — no pawn; PlayerControllerClass set in Q2's BP
}
```

### 🔵 Blueprint to create
- **`BP_DeepDelveGameMode`** (parent `ADeepDelveGameMode`). Set **Project Settings → Maps & Modes → Default GameMode = `BP_DeepDelveGameMode`** (and in the `Mine` map's World Settings).

### 🎛️ UPROPERTY → values
*(none this quest)*

### Steps
1. Install UE 5.8, **JetBrains Rider**, and the compiler toolchain (Windows: *Visual Studio Build Tools* C++ workload; macOS: Xcode command-line tools).
2. **New project → Games → Blank → C++**, Desktop, Starter Content off, name `DeepDelve`.
3. **Editor Preferences → Source Code → Source Code Editor = Rider**; build the empty project from Rider and confirm the editor opens.
4. **Migrate Synty:** open the Synty pack project, right-click its Content folder → **Asset Actions → Migrate** → choose your `DeepDelve/Content`.
5. Compose the `Mine` level (tiles + dwarf + light + a **Camera Actor**, *Auto Activate for Player 0*).
6. **Tools → New C++ Class → GameModeBase** → `DeepDelveGameMode`; paste; build. Create `BP_DeepDelveGameMode`; set it as the Default GameMode.

🏁 **Milestone:** clean compile from Rider; the `Mine` scene plays through the camera with your GameMode active.
🧪 **Boss check:** add `UE_LOG(LogTemp, Warning, TEXT("GameMode up"))` in the constructor and see it in the Output Log.
🎁 **Reward:** Title **Prospector** · +100 XP.

---

# QUEST 2 — The Player & Enhanced Input · ~55 min

**Goal.** Create the Player Controller in C++, give it the player's `Damage` stat, and wire **Enhanced Input** in code so a left-click fires an `OnDig` handler.

📚 **Teaches:** Player Controllers, and wiring player input with the Enhanced Input system in C++.
**Resources:** [Gameplay Framework (Controllers)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) · [Programming with C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · *Further:* [Tom Looman — Unreal C++](https://tomlooman.com/)

🎮 **After this quest:** clicking logs a "dig" from your C++ controller; `Damage` lives on the controller and is set in its Blueprint.

💡 **Why:** The [**PlayerController**](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) represents the player's *will* (input → commands) and persists independent of any pawn — the natural home for `Damage` and input. [**Enhanced Input**](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine) is UE5's data-driven, remappable input system that replaces the legacy mappings.

🏛️ **Patterns & principles:** **Single Responsibility** again — input handling and player stats belong to the controller, not the GameMode or the level. Note the **forward declarations** (`class UInputMappingContext;`) in the header — declaring instead of `#include`-ing keeps compile times and header coupling down, a habit the [Coding Standard](https://dev.epicgames.com/documentation/en-us/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine) encourages.

🔑 **Key nodes / functions:** `New C++ Class (PlayerController) · Build.cs: PublicDependencyModuleNames += "EnhancedInput" · ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem> · AddMappingContext · UEnhancedInputComponent::BindAction · SetInputMode(FInputModeGameAndUI) · bShowMouseCursor`

### 🧩 C++ classes & methods
**Create `ADeepDelvePlayerController : APlayerController`.**
```cpp
// header
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Stats") float Damage = 1.f;
UPROPERTY(EditDefaultsOnly, Category="Input") TObjectPtr<UInputMappingContext> MiningContext;
UPROPERTY(EditDefaultsOnly, Category="Input") TObjectPtr<UInputAction> DigAction;
virtual void BeginPlay() override;
virtual void SetupInputComponent() override;
void OnDig();
// .cpp
void ADeepDelvePlayerController::BeginPlay(){
    Super::BeginPlay(); bShowMouseCursor = true;
    if (auto* S = ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem>(GetLocalPlayer()))
        if (MiningContext) S->AddMappingContext(MiningContext, 0);
    SetInputMode(FInputModeGameAndUI());
}
void ADeepDelvePlayerController::SetupInputComponent(){
    Super::SetupInputComponent();
    if (auto* EIC = Cast<UEnhancedInputComponent>(InputComponent))
        if (DigAction) EIC->BindAction(DigAction, ETriggerEvent::Triggered, this, &ADeepDelvePlayerController::OnDig);
}
void ADeepDelvePlayerController::OnDig(){ UE_LOG(LogTemp, Warning, TEXT("Dig! %.0f"), Damage); } // Q3 traces + damages
```
Add `"EnhancedInput"` to `PublicDependencyModuleNames` in `DeepDelve.Build.cs`. `MiningContext`/`DigAction` are `TObjectPtr` **`UPROPERTY`s**, so the GC keeps the assigned assets alive.

### 🔵 Blueprints to create
- **`BP_MineController`** (parent `ADeepDelvePlayerController`). Create input assets **`IMC_Mining`** + **`IA_Dig`** (map `IA_Dig` → Left Mouse Button in `IMC_Mining`). Set **`BP_DeepDelveGameMode` → Player Controller Class = `BP_MineController`**.

### 🎛️ UPROPERTY → values (on `BP_MineController`)
| UPROPERTY | Assign |
|---|---|
| `MiningContext` | `IMC_Mining` |
| `DigAction` | `IA_Dig` |
| `Damage` | `1.0` |

### Steps
1. New C++ class → PlayerController → `DeepDelvePlayerController`; paste; add `EnhancedInput` to `Build.cs`; build.
2. Create `IMC_Mining` + `IA_Dig`; map Left Mouse Button.
3. Create `BP_MineController`, assign the table; point the GameMode's Player Controller Class at it.
4. Play, click, watch the log.

🏁 **Milestone:** left-click prints `Dig! 1` from C++.
🧪 **Boss check:** set `Damage = 5` on `BP_MineController` → the log reads 5.
🎁 **Reward:** Title **Pit Digger** · +150 XP.

---

# QUEST 3 — The Rock & the Interface · ~55 min

**Goal.** A reusable **`IMineable`** interface, the **`AOreVein`** actor that implements it, the controller's cursor trace that calls `TakeDamage`, and the first hit/break sounds.

📚 **Teaches:** C++ interfaces, Actors, decoupled damage through one shared interface, playing sounds — and the GC/`UPROPERTY` rule for object references.
**Resources:** [Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) · [Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [Unreal Object Handling (GC)](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-object-handling-in-unreal-engine) · [Object Pointers (TObjectPtr)](https://dev.epicgames.com/documentation/en-us/unreal-engine/object-pointers-in-unreal-engine) · [Audio Engine Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine) · *Further:* [Actor Lifecycle](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-actor-lifecycle)

🎮 **After this quest:** clicking the rock damages it via the interface; at 0 HP it breaks (with sound) and a fresh rock spawns.

💡 **Why:** The rock is a spawnable, tickable world object → an [**Actor**](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine). The `IMineable` interface lets the caller deal damage without knowing the concrete type, so clicks now — and dwarves/auto-swing later — all use one `TakeDamage` call with zero extra coupling.

🏛️ **Patterns & principles:**
- **[Dependency Inversion](https://en.wikipedia.org/wiki/Dependency_inversion_principle) via an [Interface](https://refactoring.guru/design-patterns)** — the controller depends on the abstraction `IMineable`, not on `AOreVein`. This is the single most reused decision in the project; every future damage source rides it.
- **Memory safety / GC** — this quest introduces the first UObject references (`Mesh`, `HitSound`, `RespawnClass`). **Every one is a `UPROPERTY()`** so Unreal's garbage collector tracks it; a raw `USoundBase*` without `UPROPERTY()` can be collected and crash. See [Unreal Object Handling](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-object-handling-in-unreal-engine). Use `TObjectPtr<T>` for members (per [Object Pointers](https://dev.epicgames.com/documentation/en-us/unreal-engine/object-pointers-in-unreal-engine)).
- **`BlueprintNativeEvent`** — the interface method has a C++ default (`_Implementation`) and stays overridable in Blueprints.

🔑 **Key nodes / functions:** `New C++ Class→Unreal Interface · UINTERFACE(BlueprintType) · BlueprintNativeEvent · TakeDamage_Implementation · GetHitResultUnderCursor(ECC_Visibility) · Implements<UMineable>() · Execute_TakeDamage · UGameplayStatics::PlaySound2D · SpawnActor/Destroy`

### 🧩 C++ classes & methods
**Interface `UMineable`/`IMineable`:**
```cpp
UINTERFACE(BlueprintType) class UMineable : public UInterface { GENERATED_BODY() };
class IMineable { GENERATED_BODY()
public: UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category="Mining") void TakeDamage(float Amount); };
```
**`AOreVein : AActor, public IMineable`** — note every reference is a `UPROPERTY`/`TObjectPtr`:
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<UStaticMeshComponent> Mesh;
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Rock") float MaxHP = 10.f;
UPROPERTY(VisibleInstanceOnly, BlueprintReadOnly, Category="Rock") float HP = 10.f;
UPROPERTY(EditAnywhere, Category="Rock|FX") TObjectPtr<USoundBase> HitSound, BreakSound;
UPROPERTY(EditAnywhere, Category="Rock") TSubclassOf<AOreVein> RespawnClass;
virtual void BeginPlay() override;                     // HP = MaxHP
virtual void TakeDamage_Implementation(float Amount) override;
// TakeDamage_Implementation: HP -= Amount; PlaySound2D(HitSound);
//   if (HP<=0){ PlaySound2D(BreakSound); SpawnActor<AOreVein>(RespawnClass, GetActorTransform()); Destroy(); }
```
**Update `ADeepDelvePlayerController::OnDig`:**
```cpp
FHitResult Hit;
if (GetHitResultUnderCursor(ECC_Visibility, false, Hit))
    if (AActor* A = Hit.GetActor()) if (A->Implements<UMineable>())
        IMineable::Execute_TakeDamage(A, Damage);
```

### 🔵 Blueprint to create
- **`BP_OreVein`** (parent `AOreVein`); place in the `Mine` level. Import `S_RockHit`, `S_RockBreak`.

### 🎛️ UPROPERTY → values (on `BP_OreVein`)
| UPROPERTY | Assign |
|---|---|
| `Mesh` (component) | a Synty rock mesh |
| `MaxHP` | `10` |
| `HitSound` / `BreakSound` | `S_RockHit` / `S_RockBreak` |
| `RespawnClass` | `BP_OreVein` (itself) |

### Steps
1. New C++ class → Unreal Interface → `Mineable`; paste.
2. New C++ class → Actor → `OreVein` (add `public IMineable`); paste; build.
3. Update `OnDig` to trace + `Execute_TakeDamage`.
4. Create `BP_OreVein`, assign the table, place it in the level.

🏁 **Milestone:** ten clicks break the rock (with sound) and a new one appears.
🧪 **Boss check:** set `MaxHP = 20` in the BP → it takes 20 clicks.
🎁 **Reward:** Title stays **Pit Digger** · +200 XP.
💡 **Notes & gotchas:** the rock needs collision the cursor trace can hit (default Synty mesh collision blocking Visibility is fine). Guard interface calls with `Implements<UMineable>()` so a mis-click on scenery is a no-op, not a crash.

---

# QUEST 4 — Coins & the Economy Subsystem · ~60 min

**Goal.** A **`UGameInstanceSubsystem`** for the economy, a collectible **`ACoin`**, a C++ **HUD widget** with `BindWidget`, and floating damage numbers pushed through an event.

📚 **Teaches:** GameInstance Subsystems, the Observer pattern (event dispatchers), collectible Actors, C++ UMG widgets with BindWidget, and decoupling UI from gameplay.
**Resources:** [Programming Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) · [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [Observer pattern](https://gameprogrammingpatterns.com/observer.html) · [Service Locator pattern](https://gameprogrammingpatterns.com/service-locator.html) · [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine)

🎮 **After this quest:** broken rocks burst coins you sweep up (ching); a HUD shows Coins + Depth; a "1" floats off each hit.

💡 **Why:** Coins/Depth must survive level changes, so they belong to the [GameInstance's](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) lifetime — a [**Subsystem**](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) is the auto-created, globally-reachable home. Events push HUD updates only when values change. The HUD is a C++ `UUserWidget` with `BindWidget` for type-safe access.

🏛️ **Patterns & principles:**
- **[Service Locator](https://gameprogrammingpatterns.com/service-locator.html)** — the economy is a Subsystem fetched with `GetGameInstance()->GetSubsystem<UMineEconomySubsystem>()`, giving one well-known instance instead of singletons or globals. It's also your **single source of truth** for game state.
- **[Observer / pub-sub](https://refactoring.guru/design-patterns/observer)** — the subsystem *broadcasts* `OnCoinsChanged` and `OnDamageDealt`; views subscribe. This is the key to **decoupling UI from gameplay**: gameplay code only *reports* facts and never touches widgets.
- **Model–View separation** — the C++ `UHUDWidget` holds logic; `WBP_HUD` holds layout, bound by name via `meta=(BindWidget)`.
- **[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)** — one `ReportDamage()` produces every floating number (clicks now, dwarves in Q8, crits in Q12).

🔑 **Key nodes / functions:** `UGameInstanceSubsystem · GetSubsystem<>() · DECLARE_DYNAMIC_MULTICAST_DELEGATE[_ThreeParams] + Broadcast · SimulatePhysics/AddImpulse · NotifyActorBeginCursorOver · bEnableMouseOverEvents · CreateWidget · meta=(BindWidget) · GetOwningPlayer()->ProjectWorldLocationToScreen · SetLifeSpan · Build.cs += UMG/Slate/SlateCore`

### 🧩 C++ classes & methods
**`UMineEconomySubsystem : UGameInstanceSubsystem`** (no Blueprint — auto-instantiated):
```cpp
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCoinsChanged);
DECLARE_DYNAMIC_MULTICAST_DELEGATE_ThreeParams(FOnDamageDealt, FVector, WorldLocation, float, Amount, bool, bCrit);
UPROPERTY(BlueprintReadOnly) double Coins = 0, Depth = 0;
UPROPERTY(BlueprintAssignable) FOnCoinsChanged OnCoinsChanged;
UPROPERTY(BlueprintAssignable) FOnDamageDealt OnDamageDealt;   // views subscribe; gameplay just reports
UFUNCTION(BlueprintCallable) void AddCoins(double A);          // Coins+=A; OnCoinsChanged.Broadcast();
UFUNCTION(BlueprintCallable) void AddDepth(double A);
UFUNCTION(BlueprintCallable) void ReportDamage(FVector Where, float Amount, bool bCrit); // OnDamageDealt.Broadcast(...)
```
**`UMineStatics : UBlueprintFunctionLibrary`** — `UFUNCTION(BlueprintPure) static FText FormatNumber(double Value);` (1500 → "1.5K"; `FText` keeps it localizable).
**`ACoin : AActor`** — `UPROPERTY(VisibleAnywhere) TObjectPtr<UStaticMeshComponent> Mesh;` (Simulate Physics in BP); `int32 Value=1; float LaunchImpulse=300; float Lifespan=30;` `UPROPERTY(EditAnywhere) TObjectPtr<USoundBase> PickupSound;` `BeginPlay` (random `AddImpulse`, `SetLifeSpan`); `NotifyActorBeginCursorOver` → `Collect()`; **`void Collect();`** (subsystem `AddCoins(Value)`; `PlaySound2D`; `Destroy`) — **keep `Collect()` public** (the collector reuses it in Q15).
**`UHUDWidget : UUserWidget`** — owns the damage-number view so gameplay stays UI-free:
```cpp
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UTextBlock> CoinsText;
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UTextBlock> DepthText;
UPROPERTY(EditDefaultsOnly) TSubclassOf<UUserWidget> DamageNumberClass; // WBP_DamageNumber
virtual void NativeConstruct() override;   // Econ->OnCoinsChanged.AddDynamic(this, &::Refresh);
                                           // Econ->OnDamageDealt.AddDynamic(this, &::OnDamage);
UFUNCTION() void Refresh();                // CoinsText/DepthText via UMineStatics::FormatNumber
UFUNCTION() void OnDamage(FVector Where, float Amount, bool bCrit); // ProjectWorldLocationToScreen + spawn WBP_DamageNumber
```
**Update `AOreVein`** — `UPROPERTY(EditAnywhere) int32 CoinReward=10; UPROPERTY(EditAnywhere) TSubclassOf<ACoin> CoinClass;` break branch loops `CoinReward`→`SpawnActor<ACoin>`, `Econ->AddDepth(1)`.
**Update `ADeepDelvePlayerController`** — in `BeginPlay` set `bEnableMouseOverEvents=true` and create the HUD (`UPROPERTY(EditDefaultsOnly) TSubclassOf<UHUDWidget> HUDClass;`). `OnDig` **reports** the hit: after `Execute_TakeDamage`, call `Econ->ReportDamage(Hit.ImpactPoint, Damage, false)` — the controller never spawns UI itself.

> **Build.cs:** the first `UUserWidget` (`UHUDWidget`) means adding **`"UMG"`, `"Slate"`, `"SlateCore"`** to `PublicDependencyModuleNames` — or nothing UMG-related compiles.

### 🔵 Blueprints to create
- **`BP_Coin`** (parent `ACoin`), **`WBP_HUD`** (parent `UHUDWidget`), **`WBP_DamageNumber`** (a `UUserWidget` with a float-up+fade animation).

### 🎛️ UPROPERTY → values
| Class | UPROPERTY | Assign |
|---|---|---|
| `BP_Coin` | `Mesh` | Synty coin mesh; **Simulate Physics = true** |
| `BP_Coin` | `Value` / `PickupSound` | `1` / `S_CoinPickup` |
| `WBP_HUD` | `CoinsText`, `DepthText` | Text Blocks named exactly `CoinsText`/`DepthText` |
| `WBP_HUD` | `DamageNumberClass` | `WBP_DamageNumber` |
| `BP_MineController` | `HUDClass` | `WBP_HUD` |
| `BP_OreVein` | `CoinReward` / `CoinClass` | `10` / `BP_Coin` |

### Steps
1. Create the subsystem (with both delegates + `ReportDamage`), `UMineStatics`, `ACoin`, `UHUDWidget`; add `UMG`/`Slate`/`SlateCore` to `Build.cs`; build.
2. Update `AOreVein` (coin fields + spawn loop) and the controller (mouse-over + HUD + `ReportDamage`).
3. Create `BP_Coin`, `WBP_HUD` (match `BindWidget` names, assign `DamageNumberClass`), `WBP_DamageNumber`; assign the table.
4. Play; break rocks; collect coins.

🏁 **Milestone:** break → coins burst → hover to collect → HUD climbs, Depth ticks, damage numbers float.
🧪 **Boss check:** a coin credits exactly once and the HUD total matches.
🎁 **Reward:** Title **Miner** · +200 XP.
💡 **Notes & gotchas:** `BindWidget` requires the `WBP_HUD` widget names to **exactly match** the C++ members. Coins auto-clean via `SetLifeSpan`. **Optimization to remember (Q16):** spawning/destroying many coins churns memory — an [Object Pool](https://gameprogrammingpatterns.com/object-pool.html) reuses them (see [UE performance considerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-memory-and-cpu-performance-considerations-in-unreal-engine)). Consider caching the subsystem pointer instead of re-fetching it each hit.

---

# QUEST 5 — Data-Driven Content · ~50 min

**Goal.** Move rock and pickaxe stats into **`UPrimaryDataAsset`s**, and route the active rock/pickaxe through the subsystem so Descend (Q6) can swap tiers.

📚 **Teaches:** data-driven design with Data Assets (`UPrimaryDataAsset`) so content is authored, not coded.
**Resources:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Type Object pattern](https://gameprogrammingpatterns.com/type-object.html) · *Further:* [Data-Driven Gameplay Elements](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-driven-gameplay-elements-in-unreal-engine)

🎮 **After this quest:** identical to play, but a new rock/pickaxe is a new `DA_` asset — no recompile.

💡 **Why:** Hard-coding stats means every new rock is a code change. [**Data Assets**](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) separate data from behavior; routing the active rock/pickaxe through the subsystem gives one source of truth the tech tree can change at runtime.

🏛️ **Patterns & principles:** the **[Type Object pattern](https://gameprogrammingpatterns.com/type-object.html)** — instead of a subclass per rock, one `AOreVein` reads its stats from a `URockData` "type" asset. Designers create content as data; the code stays fixed. This is the data-driven backbone the whole tech tree (Q6) builds on.

🔑 **Key nodes / functions:** `UPrimaryDataAsset · right-click→Miscellaneous→Data Asset · TObjectPtr<> asset refs · SetStaticMesh · GetSubsystem`

### 🧩 C++ classes & methods
**`URockData : UPrimaryDataAsset`** — `UPROPERTY(EditAnywhere) TObjectPtr<UStaticMesh> Mesh; int32 MaxHP=10; int32 CoinReward=10;`
**`UPickaxeData : UPrimaryDataAsset`** — `UPROPERTY(EditAnywhere) TObjectPtr<UStaticMesh> Mesh; float Damage=1; float Speed=1;`
**Update `AOreVein`** — data-driven, seeded through the subsystem:
```cpp
UPROPERTY(EditAnywhere, Category="Rock") TObjectPtr<URockData> RockData; // starting tier (BP-assigned)
// BeginPlay:
//   auto* Econ = GetGameInstance()->GetSubsystem<UMineEconomySubsystem>();
//   if (!Econ->CurrentRock) Econ->CurrentRock = RockData;   // seed once
//   const URockData* A = Econ->CurrentRock;
//   Mesh->SetStaticMesh(A->Mesh); MaxHP=A->MaxHP; CoinReward=A->CoinReward; HP=MaxHP;
//   Econ->CurrentRockActor=this; Econ->OreVeinClass=GetClass(); Econ->LastRockTransform=GetActorTransform();
```
**Add to `UMineEconomySubsystem`** — all references are `UPROPERTY()` so the GC tracks them:
```cpp
UPROPERTY() TObjectPtr<URockData> CurrentRock;
UPROPERTY() TObjectPtr<AActor> CurrentRockActor;
UPROPERTY() TSubclassOf<AOreVein> OreVeinClass;
UPROPERTY() FTransform LastRockTransform;
UPROPERTY(BlueprintReadOnly) TObjectPtr<UPickaxeData> EquippedPickaxe;
```
**Update `ADeepDelvePlayerController`:** `UPROPERTY(EditDefaultsOnly) TObjectPtr<UPickaxeData> DefaultPickaxe;` + `float GetDamage() const;` (= `Econ->EquippedPickaxe->Damage`; Q6 adds `+ Econ->PickaxeDamageBonus`). In `BeginPlay`: `if(!Econ->EquippedPickaxe) Econ->EquippedPickaxe=DefaultPickaxe;` `OnDig` sends `GetDamage()`.

### 🔵 Data Assets & Blueprint values
- `DA_CopperVein` (`URockData`: 10/10), `DA_IronVein` (25/30), `DA_StonePick` (`UPickaxeData`: Damage 1), `DA_SturdyPick` (Damage 3).

| Class | UPROPERTY | Assign |
|---|---|---|
| `BP_OreVein` | `RockData` | `DA_CopperVein` |
| `BP_MineController` | `DefaultPickaxe` | `DA_StonePick` |

### Steps
1. Create `URockData`, `UPickaxeData`; build; make the four `DA_` instances.
2. Add the subsystem fields (all `UPROPERTY()`); update `AOreVein::BeginPlay` and the controller's damage source.
3. Assign `RockData`/`DefaultPickaxe` in the BPs.
4. Prove it: point `RockData` at `DA_IronVein` → tougher/richer with no recompile.

🏁 **Milestone:** a new rock type + pickaxe added purely as `DA_` assets.
🧪 **Boss check:** create a third `URockData` in under two minutes and slot it in.
🎁 **Reward:** Title stays **Miner** · +200 XP.
💡 **Notes & gotchas:** `CurrentRockActor` is a `UPROPERTY()` `TObjectPtr` — when the rock is destroyed the GC auto-nulls it, so always guard with `IsValid(Econ->CurrentRockActor)` before using it (dwarves/auto-swing do this from Q8).

---

# QUEST 6 — Tech Tree: the Model · ~65 min

**Goal.** A data-driven tech tree with a **polymorphic `UTechEffect`** hierarchy driving every upgrade; all modifiers live on the economy subsystem.

📚 **Teaches:** a polymorphic `UObject` effect system (the Strategy pattern) and a data-driven tech-tree model that obeys the Open/Closed principle.
**Resources:** [Objects (UObject)](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects-in-unreal-engine) · [Strategy pattern](https://refactoring.guru/design-patterns/strategy) · [Open/Closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** buy the first three nodes (temporary button): Sharpen → Iron Pickaxe → Descend — and descending swaps the rock.

💡 **Why:** Every upgrade does something different, but the tree shouldn't care. Each node owns a [**`UObject`**](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects-in-unreal-engine) effect with a virtual `Apply()`, so adding an upgrade is adding a class, not editing a growing switch.

🏛️ **Patterns & principles:**
- **[Strategy pattern](https://refactoring.guru/design-patterns/strategy)** — each `UTechEffect` subclass encapsulates one interchangeable behaviour behind a common `Apply()` interface.
- **[Open/Closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)** — the tree is *open* to new effects (add a subclass) and *closed* to modification (no existing code changes). The Q6 boss check verifies exactly this.
- **Composition over inheritance** — a node *has-an* effect (instanced `UObject`) rather than the node hierarchy exploding into one subclass per upgrade.

🔑 **Key nodes / functions:** `UCLASS(Abstract, EditInlineNew, DefaultToInstanced) · virtual Apply() · UPROPERTY(Instanced) · TMap · FMath::Pow · GetActorOfClass(ACameraActor) · AddActorWorldOffset · SpawnActor<AOreVein>`

### 🧩 C++ classes & methods
**`UTechEffect : UObject`** (base):
```cpp
UCLASS(Abstract, EditInlineNew, DefaultToInstanced, BlueprintType)
class DEEPDELVE_API UTechEffect : public UObject { GENERATED_BODY()
public: UFUNCTION() virtual void Apply(class UMineEconomySubsystem* Econ, int32 NewLevel) {} };
```
**Subclasses:** `UTechEffect_UpgradePickaxeDamage` (`float AmountPerLevel=1;` → `Econ->PickaxeDamageBonus += AmountPerLevel`); `UTechEffect_EquipPickaxe` (`UPROPERTY(EditAnywhere) TObjectPtr<UPickaxeData> Pickaxe;` → `Econ->EquippedPickaxe = Pickaxe`); `UTechEffect_Descend` (`UPROPERTY(EditAnywhere) TObjectPtr<URockData> NextRock;` → `Econ->SetCurrentRock(NextRock)`).
**`UTechNode : UPrimaryDataAsset`** — `FName NodeId; FText DisplayName, Description; TObjectPtr<UTexture2D> Icon; int32 Cost=10; float CostGrowth=1.15f; bool bRepeatable=false; int32 MaxLevel=1; TArray<TObjectPtr<UTechNode>> Prerequisites; FVector2D GridPosition; UPROPERTY(Instanced) TObjectPtr<UTechEffect> Effect;` (all `UPROPERTY(EditAnywhere)`).
**Update `UMineEconomySubsystem`** (`EquippedPickaxe`, `CurrentRock`, `CurrentRockActor`, `OreVeinClass`, `LastRockTransform` exist from Q5, all `UPROPERTY()`):
```cpp
UPROPERTY() TMap<FName,int32> NodeLevels;
UPROPERTY(BlueprintReadOnly) float PickaxeDamageBonus = 0.f;   // UpgradePickaxeDamage adds here
UPROPERTY(EditAnywhere) float DescendCameraStep = 150.f;       // tunable in the subsystem's config
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnTechChanged);
UPROPERTY(BlueprintAssignable) FOnTechChanged OnTechChanged;

UFUNCTION(BlueprintCallable) int32 GetNodeLevel(UTechNode* Node) const;
UFUNCTION(BlueprintCallable) int32 GetCost(UTechNode* Node) const;   // Cost * CostGrowth^level
UFUNCTION(BlueprintCallable) bool  CanBuy(UTechNode* Node) const;    // prereqs met, not maxed, Coins>=cost
UFUNCTION(BlueprintCallable) bool  TryBuy(UTechNode* Node);          // spend, level++, Effect->Apply, broadcast
UFUNCTION() void SetCurrentRock(URockData* NewRock);                 // called by UTechEffect_Descend
```
`GetDamage()` (on the controller) now returns `EquippedPickaxe->Damage + PickaxeDamageBonus`.

**`SetCurrentRock` makes descending actually change the rock** — guard the actor pointer:
```cpp
void UMineEconomySubsystem::SetCurrentRock(URockData* NewRock)
{
    CurrentRock = NewRock;
    const FTransform Where = LastRockTransform;
    if (IsValid(CurrentRockActor)) CurrentRockActor->Destroy();     // guarded; GC nulls it after
    GetWorld()->SpawnActor<AOreVein>(OreVeinClass, Where);          // new rock reads CurrentRock in BeginPlay
    if (AActor* Cam = UGameplayStatics::GetActorOfClass(this, ACameraActor::StaticClass()))
        Cam->AddActorWorldOffset(FVector(0, 0, -DescendCameraStep));
}
```

### 🔵 Data Assets (effects are inline on the node — no BP subclass)
- `DA_Node_Sharpen`, `DA_Node_IronPick`, `DA_Node_Descend`.

### 🎛️ UPROPERTY → values
| Data Asset | Field | Value |
|---|---|---|
| `DA_Node_Sharpen` | `bRepeatable`/`Cost`/`Effect` | `true`/`10`/`UpgradePickaxeDamage(AmountPerLevel 1)` |
| `DA_Node_IronPick` | `Prerequisites`/`Cost`/`Effect` | `[Sharpen]`/`100`/`EquipPickaxe(DA_SturdyPick)` |
| `DA_Node_Descend` | `Prerequisites`/`Cost`/`Effect` | `[IronPick]`/`250`/`Descend(DA_IronVein)` |
| each node | `Icon` | a placeholder texture or mesh-captured thumbnail |

### Steps
1. New C++ class → Object → `TechEffect` (`Abstract, EditInlineNew, DefaultToInstanced`); add `virtual Apply`.
2. Add the three effect subclasses with params + `Apply` overrides.
3. New C++ class → PrimaryDataAsset → `TechNode`; mark `Effect` `Instanced`.
4. Extend the subsystem (`NodeLevels`, `PickaxeDamageBonus`, `OnTechChanged`, `GetCost/CanBuy/TryBuy`, `SetCurrentRock`).
5. Create the three `DA_Node_*`, set effects/prereqs/costs.
6. Wire a temporary button to `TryBuy`; confirm the three nodes work (descend swaps the rock); build.

🏁 **Milestone:** buying the three nodes sharpens, equips, and descends to tougher/richer rock.
🧪 **Boss check:** add a 4th node as a `DA_` + one new effect subclass — **with zero edits to any existing effect** (open/closed in action).
🎁 **Reward:** Title **Pit Foreman** · +300 XP.
💡 **Notes & gotchas:** mark `UTechEffect` `EditInlineNew`+`DefaultToInstanced` and the node's `Effect` `Instanced`, or you can't create effect objects inline on the Data Asset. ⚖️ `CostGrowth` (~1.10–1.15) sets pacing — expose tuning values as `EditAnywhere` so you balance without recompiling; tune after the demo.

---

# QUEST 7 — Tech Tree: the UI · ~60 min

**Goal.** The real tech-tree screen from C++ `UUserWidget` bases + `WBP_` layouts.

📚 **Teaches:** building data-driven UMG screens in C++ — dynamic widgets on a Canvas with custom-drawn connectors.
**Resources:** [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) · *Further:* [UMG UI Designer Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/umg-ui-designer-quick-start-guide-in-unreal-engine)

🎮 **After this quest:** a screen of connected nodes that light up when affordable and check off when bought.

💡 **Why:** Building the screen from the node data means the UI reflects the tree automatically. C++ holds the logic (buy/refresh/connectors); the WBP supplies the look — the same split as the HUD.

🏛️ **Patterns & principles:** **Model–View separation** taken further — the widgets are pure *view*, reading from the `UTechNode` data (model) and the subsystem (state), and they refresh reactively off `OnTechChanged` (**Observer** again). No game rules live in the UI.

🔑 **Key nodes / functions:** `UUserWidget · meta=(BindWidget) UButton/UImage/UTextBlock · UCanvasPanel · CreateWidget (in a loop) · UCanvasPanelSlot::SetPosition · NativePaint → UWidgetBlueprintLibrary::DrawLine · Button->OnClicked.AddDynamic · SetIsEnabled`

### 🧩 C++ classes & methods
**`UTechNodeWidget : UUserWidget`** — `UPROPERTY(meta=(BindWidget)) TObjectPtr<UButton> BuyButton; TObjectPtr<UImage> IconImage; TObjectPtr<UTextBlock> NameText,CostText;` `UPROPERTY() TObjectPtr<UTechNode> Node;` `void Setup(UTechNode*)` (fill icon/name; `BuyButton->OnClicked.AddDynamic(this,&…::OnBuyClicked)`); `UFUNCTION() void OnBuyClicked()` (`Econ->TryBuy(Node)`); `UFUNCTION() void Refresh()` (colour by `CanBuy`/owned/maxed; `SetIsEnabled`).
**`UTechTreeWidget : UUserWidget`** — `UPROPERTY(meta=(BindWidget)) TObjectPtr<UCanvasPanel> Canvas;` `UPROPERTY(EditDefaultsOnly) TSubclassOf<UTechNodeWidget> NodeWidgetClass; UPROPERTY(EditDefaultsOnly) TArray<TObjectPtr<UTechNode>> AllNodes;` `NativeConstruct` (spawn a node widget per `AllNodes` at `GridPosition`; bind `OnTechChanged`→RefreshAll); `NativePaint` (draw connector lines from each node's Prerequisites).

### 🔵 Blueprints
- **`WBP_TechNode`** (widgets named `BuyButton`,`IconImage`,`NameText`,`CostText`), **`WBP_TechTree`** (Canvas named `Canvas`).

### 🎛️ UPROPERTY → values
| Widget | Field | Value |
|---|---|---|
| `WBP_TechTree` | `NodeWidgetClass` | `WBP_TechNode` |
| `WBP_TechTree` | `AllNodes` | `[DA_Node_Sharpen, DA_Node_IronPick, DA_Node_Descend]` |

### Steps
1. New UserWidget `TechNodeWidget` (BindWidget members + Setup/OnBuyClicked/Refresh).
2. New UserWidget `TechTreeWidget` (Canvas + NodeWidgetClass + AllNodes + NativeConstruct + NativePaint).
3. Create `WBP_TechNode` (matching names) + `WBP_TechTree` (Canvas named `Canvas`); assign the table.
4. Add the tree widget to the viewport (from the controller or a "tech tree" button); build; buy nodes.

🏁 **Milestone:** the visual tree; click affordable nodes to buy.
🧪 **Boss check:** add the 4th node's `GridPosition`+prereq — appears wired with no layout code.
🎁 **Reward:** Title stays **Pit Foreman** · +300 XP.
💡 **Notes & gotchas:** node `Icon` (`UTexture2D`) can be a placeholder or a mesh-captured thumbnail.

---

# QUEST 8 — Hire the Crew · ~60 min

**Goal.** An animated `ADwarf` that mines the rock on a timer via `IMineable`, hired through a tech node.

📚 **Teaches:** spawning animated Actors, gameplay timers, and reusing a gameplay interface for autonomous behavior.
**Resources:** [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Update Method vs timers](https://gameprogrammingpatterns.com/update-method.html) · [Interface reuse](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)

🎮 **After this quest:** buy "Hire Dwarf" → a dwarf appears and mines the rock on his own.

💡 **Why:** A dwarf is a visible, spawnable, animated Actor. He mines through the same `IMineable` interface your clicks use — the payoff of Q3: idle income needs no new coupling. A looping Timer drives the swing (fixed cadence, cheaper than per-frame Tick).

🏛️ **Patterns & principles:**
- **Dependency Inversion in action** — the dwarf deals damage via `IMineable::Execute_TakeDamage`, exactly like the player. Adding autonomous actors required *zero* changes to `AOreVein`.
- **[Update Method vs Timers](https://gameprogrammingpatterns.com/update-method.html)** — a swing is periodic, not per-frame, so a looping **[Gameplay Timer](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine)** is the right tool (leave `Tick` disabled). You'll contrast this with the collector's per-frame `Tick` in Q15.
- **Observer reuse** — the dwarf *reports* its damage via `Econ->ReportDamage(...)`; the HUD shows the number. The dwarf never touches UI.

🔑 **Key nodes / functions:** `USkeletalMeshComponent · Anim asset / PlayAnimation · GetWorld()->GetTimerManager().SetTimer(handle, this, &..::Swing, Interval, true) · IsValid() · Execute_TakeDamage · Econ->ReportDamage · SpawnActor`

### 🧩 C++ classes & methods
**`UMinerData : UPrimaryDataAsset`** — `UPROPERTY(EditAnywhere) TObjectPtr<USkeletalMesh> Mesh; TObjectPtr<UAnimationAsset> MineAnim; float Damage; float Speed;`
**`ADwarf : AActor`** — every reference is a `UPROPERTY`:
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<USkeletalMeshComponent> Mesh;
UPROPERTY(EditAnywhere) TObjectPtr<UMinerData> MinerData;
FTimerHandle SwingTimer;
virtual void BeginPlay() override;  // set mesh + play MineAnim; GetTimerManager().SetTimer(SwingTimer, this, &ADwarf::Swing, MinerData->Speed, true)
void Swing();
// Swing(): auto* R = Econ->CurrentRockActor; if (!IsValid(R)) return;      // guard the respawn gap
//   const float Dmg = MinerData->Damage * Econ->CrewDamageMult;
//   IMineable::Execute_TakeDamage(R, Dmg); Econ->ReportDamage(R->GetActorLocation(), Dmg, false);
```
**`UTechEffect_HireMiner : UTechEffect`** — `UPROPERTY(EditAnywhere) TObjectPtr<UMinerData> Miner; TSubclassOf<ADwarf> DwarfClass;` → `Econ->SpawnDwarf(DwarfClass, Miner)`.
**Update `UMineEconomySubsystem`** — `UPROPERTY() float CrewDamageMult = 1.f; UPROPERTY() TArray<TObjectPtr<UMinerData>> HiredMiners; UPROPERTY() TSubclassOf<ADwarf> DwarfClass;` and `void SpawnDwarf(TSubclassOf<ADwarf> Class, UMinerData* Miner)` — spawn at a ring slot **and record** `DwarfClass = Class; HiredMiners.Add(Miner);` so Q10 can rebuild the exact crew.

### 🔵 Blueprints / Data
- **`BP_Dwarf`** (parent `ADwarf`), **`DA_Dwarf1`** (`UMinerData`), **`DA_Node_HireDwarf`** (`UTechNode`, repeatable).

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Dwarf1` | `Mesh`/`MineAnim`/`Damage`/`Speed` | Synty dwarf mesh / mine anim / `1` / `1.0` |
| `BP_Dwarf` | `MinerData` | `DA_Dwarf1` |
| `DA_Node_HireDwarf` | `Effect` | `HireMiner(Miner=DA_Dwarf1, DwarfClass=BP_Dwarf)` |

### Steps
1. New PrimaryDataAsset `MinerData`; New Actor `ADwarf` (mesh + timer + `Swing` with the `IsValid` guard).
2. New `UTechEffect_HireMiner`; extend the subsystem (`CrewDamageMult`, `HiredMiners`, `DwarfClass`, `SpawnDwarf`).
3. Create `BP_Dwarf`, `DA_Dwarf1`, `DA_Node_HireDwarf`; assign values.
4. Build; buy Hire Dwarf; watch it mine.

🏁 **Milestone:** a hired dwarf mines hands-free.
🧪 **Boss check:** hire two; both damage the rock independently.
🎁 **Reward:** Title **Master Smith** · +300 XP.
💡 **Notes & gotchas:** always `IsValid(Econ->CurrentRockActor)` before hitting it — between a rock breaking and its replacement's `BeginPlay`, the pointer is briefly null (the GC nulled the `UPROPERTY`). ⭐ *Optional:* deal damage on the exact swing frame with an [Anim Notify](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine).

---

# QUEST 9 — Menus & Level Flow · ~55 min

**Goal.** A Main Menu level and an Esc pause menu, driven by a C++ `UMenuWidget`.

📚 **Teaches:** level management (Open Level), UMG menus, input modes, and pausing.
**Resources:** [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Game Mode & Game State](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) · [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine)

🎮 **After this quest:** the game boots to a title screen (New Game / Quit); Esc pauses in the mine.

💡 **Why:** Splitting the title and game into levels keeps concerns apart; `OpenLevel` swaps them while the GameInstance (and economy subsystem) persists — which is why the economy lives there. The menu gets its own GameMode so the mining controller/HUD never run on the title screen.

🏛️ **Patterns & principles:** **Separation of concerns across levels** — a dedicated menu GameMode means the menu never accidentally runs mining logic. It also demonstrates **lifetime scoping**: level-scoped actors reset on `OpenLevel`, while GameInstance-scoped state (your subsystem) survives — the reason New Game must *explicitly* `ResetToDefaults()`.

🔑 **Key nodes / functions:** `UGameplayStatics::OpenLevel(FName) · UKismetSystemLibrary::QuitGame · SetInputMode(FInputModeUIOnly) · Set Game Paused · Level Blueprint BeginPlay · World Settings→GameMode Override · Game Default Map · IA_Pause`

### 🧩 C++ classes & methods
**`UMenuWidget : UUserWidget`**
```cpp
UFUNCTION(BlueprintCallable) void NewGame();     // Econ->ResetToDefaults(); UGameplayStatics::OpenLevel(this,"Mine")
UFUNCTION(BlueprintCallable) void Continue();    // Q9: hidden/disabled (no save yet) — implemented in Q10
UFUNCTION(BlueprintCallable) void QuitToMenu();  // OpenLevel(this,"MainMenu")
UFUNCTION(BlueprintCallable) void QuitGame();    // UKismetSystemLibrary::QuitGame(...)
```
**Update `ADeepDelvePlayerController`** — add `IA_Pause`; on it create/remove a pause widget, `SetInputMode`, `Set Game Paused`; expose `UPROPERTY(EditDefaultsOnly) TSubclassOf<UMenuWidget> PauseMenuClass`.
**Main menu display (concrete owner):** give the **MainMenu level a GameMode Override = `BP_MenuGameMode`** (parent `AGameModeBase`, stock PlayerController) so the mining controller/HUD never run there; the MainMenu **Level Blueprint** on BeginPlay creates `WBP_MainMenu` → Add to Viewport → Set Input Mode UI Only → Show Mouse Cursor.
**Update `UMineEconomySubsystem`** — `void ResetToDefaults()` (clears Coins/Depth/NodeLevels, destroys dwarves, clears `HiredMiners`, resets multipliers and `CurrentRock`).

### 🔵 Blueprints / Levels
- **`WBP_MainMenu`**, **`WBP_PauseMenu`** (parent `UMenuWidget`), **`BP_MenuGameMode`** (parent `AGameModeBase`), a **MainMenu** level (World Settings → GameMode Override = `BP_MenuGameMode`). Set **Game Default Map = MainMenu**.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_MineController` | `IA_Pause` / `PauseMenuClass` | `IA_Pause` (Esc) / `WBP_PauseMenu` |

### Steps
1. New UserWidget `UMenuWidget` with the four actions (Continue hidden for now).
2. Add `IA_Pause` + pause logic + `PauseMenuClass` to the controller; `ResetToDefaults` on the subsystem.
3. Create `WBP_MainMenu`, `WBP_PauseMenu`, `BP_MenuGameMode`, and the MainMenu level (GameMode Override + Level BP creates the menu).
4. Set Game Default Map = MainMenu; play from the menu → New Game → Esc pause.

🏁 **Milestone:** title → New Game → mine; Esc pauses/returns.
🧪 **Boss check:** New Game twice; the second run is fresh (`ResetToDefaults`).
🎁 **Reward:** Title stays **Master Smith** · +250 XP.
💡 **Notes & gotchas:** the GameInstance persists across `OpenLevel`, so New Game must explicitly reset it.

---

# QUEST 10 — Save & Load · ~60 min

**Goal.** A `USaveGame` subclass + save/load in the subsystem that **reconstructs the world** (dwarves, tier, pickaxe).

📚 **Teaches:** the SaveGame system — serialization, soft object references, and rebuilding runtime state on load.
**Resources:** [Saving and Loading Your Game](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine)

🎮 **After this quest:** quit, relaunch, Continue → coins, upgrades, depth, and dwarves are all exactly back.

💡 **Why:** [`USaveGame`](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) is UE's built-in container for persistent progress, serialized to a slot on disk. Soft references keep a stable asset path resolved on load (no Asset Manager setup). Loading rebuilds the world because saving data isn't the same as recreating actors.

🏛️ **Patterns & principles:** a lightweight **Memento** — the SaveGame captures the state needed to restore later, and `RebuildWorld()` reconstructs the runtime *actors* from that saved *data* (a clean separation of persistent state vs live objects). **Save versioning** (`SaveVersion` + defaulting missing fields) is a forward-compatibility habit worth keeping in every project.

🔑 **Key nodes / functions:** `USaveGame · CreateSaveGameObject · SaveGameToSlot · DoesSaveGameExist · LoadGameFromSlot · DeleteGameInSlot · TSoftObjectPtr::LoadSynchronous · FDateTime::UtcNow · autosave timer`

### 🧩 C++ classes & methods
**`UDeepDelveSaveGame : USaveGame`**
```cpp
UPROPERTY() int32 SaveVersion = 1;
UPROPERTY() double Coins = 0, Depth = 0;
UPROPERTY() TMap<FName,int32> NodeLevels;
UPROPERTY() TArray<TSoftObjectPtr<UMinerData>> HiredMiners;   // respawn the exact crew
UPROPERTY() TSoftObjectPtr<URockData> CurrentRock;
UPROPERTY() TSoftObjectPtr<UPickaxeData> EquippedPickaxe;
UPROPERTY() FDateTime LastSavedTime;
```
**Update `UMineEconomySubsystem`**
```cpp
UFUNCTION(BlueprintCallable) void SaveGame();  // CreateSaveGameObject -> copy state -> SaveGameToSlot("DeepDelve",0)
UFUNCTION(BlueprintCallable) void LoadGame();  // DoesSaveGameExist -> LoadGameFromSlot -> Cast -> copy -> RebuildWorld()
UFUNCTION(BlueprintCallable) bool HasSave() const;
void RebuildWorld();  // for each HiredMiners: SpawnDwarf(DwarfClass, Miner.LoadSynchronous());
                      // set CurrentRock/EquippedPickaxe via .LoadSynchronous(); Broadcast OnTechChanged/OnCoinsChanged
```
Wire `UMenuWidget::Continue` (enabled only if `HasSave()`) → `LoadGame` then `OpenLevel("Mine")`; make `NewGame` also `DeleteGameInSlot`. Add a repeating autosave timer.

### 🔵 Blueprints
- None new (the `USaveGame` subclass is used from C++). Wire the pause-menu Save/Load and menu Continue buttons to the C++ `UFUNCTION`s.

### 🎛️ UPROPERTY → values
*(none — save state is code; slot name is a constant)*

### Steps
1. New SaveGame class `UDeepDelveSaveGame` with the fields.
2. Add `SaveGame/LoadGame/HasSave/RebuildWorld` + the autosave timer to the subsystem.
3. Implement `RebuildWorld` (respawn crew from `HiredMiners`; resolve soft refs).
4. Wire pause-menu Save/Load and enable menu Continue (`HasSave`); `NewGame` deletes the slot.
5. Test quit → relaunch → Continue.

🏁 **Milestone:** relaunch → Continue → dwarves + depth restored.
🧪 **Boss check:** delete the save; a clean first-run still works.
🎁 **Reward:** Title stays **Master Smith** · +300 XP.
💡 **Notes & gotchas:** bump `SaveVersion` and default missing fields on load — Q13–16 add fields and old saves must not crash. `LoadSynchronous()` is fine here; for larger projects prefer async streaming to avoid a hitch.

---

# QUEST 11 — Ship a Demo · ~45 min

**Goal.** Package the C++ build and send it to a friend.

📚 **Teaches:** packaging and distributing a standalone build.
**Resources:** [Packaging Your Project](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) · *Further:* [Packaging & Shipping](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-and-shipping-in-unreal-engine)

🎮 **After this quest:** a standalone Deep Delve demo a friend can run with no Unreal installed.

💡 **Why:** Shipping early gives real feedback and proves the loop runs outside the editor. Packaging produces a self-contained build; doing it now keeps any packaging issues small.

🏛️ **Patterns & principles:** *ship early, iterate.* A vertical slice that runs standalone validates your whole architecture end-to-end (input → economy → save → package) far better than any amount of in-editor testing.

🔑 **Key nodes / functions:** `Platforms→Windows→Package Project · Build Configuration: Development · Project Settings→Description (name/icon)`

### 🧩 C++ work
- None new. (A C++ project needs the toolchain to package; the packaged build is self-contained for players.)

### Steps
1. Confirm Game Default Map = MainMenu, project name/icon set.
2. **Platforms → Windows → Package Project**, config **Development**.
3. Test the `.exe`: new game → upgrade → save → quit → relaunch → Continue.
4. Zip + share (Drive/WeTransfer).

🏁 **Milestone:** a shippable demo runs a full play/save/quit/continue cycle outside the editor.
🧪 **Boss check:** run it on a clean folder or another machine.
🎁 **Reward:** Title stays **Master Smith** · +250 XP · 🎉
💡 **Notes & gotchas:** tune the economy (`Cost`/`CostGrowth` on the tech nodes) from your friend's feedback before adding content.

---

# QUEST 12 — Pickaxe Upgrades: Auto-Swing & Crits · ~55 min

**Goal.** New `UTechEffect` subclasses that add an auto-swing and critical hits, all routed through one swing path.

📚 **Teaches:** extending a system with new effect classes (no edits to old ones), randomness for crits, timers, and a single shared action path (DRY).
**Resources:** [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Programming with C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** the pickaxe swings on its own and occasionally lands a gold "CRIT!".

💡 **Why:** Manual and auto swings must behave identically, so both funnel through one `PerformSwing()` rather than duplicating logic. RNG crits add variance; the auto-swing uses a looping Timer like the dwarves.

🏛️ **Patterns & principles:**
- **[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)** — `PerformSwing()` is the *one* place a player swing happens (crit roll, damage, `ReportDamage`); the manual click and the auto-swing timer both call it. No copy-paste, no drift.
- **Open/Closed continued** — auto-swing and crits are new `UTechEffect` subclasses; nothing in Q6's tree code changes.
- **[Observer](https://gameprogrammingpatterns.com/observer.html)** — the auto-swing timer lives on the **controller** (SRP: it's the player's pickaxe), and the controller re-configures it by subscribing to the subsystem's `OnTechChanged` — so buying "Swing Speed" retunes the live timer without the subsystem knowing about the controller.

🔑 **Key nodes / functions:** `GetTimerManager().SetTimer/ClearTimer (auto-swing, on the controller) · OnTechChanged.AddDynamic · FMath::FRand() (or FRandomStream) · Econ->ReportDamage · PlaySound2D`

### 🧩 C++ classes & methods
**One swing path on the controller.** Move the manual-dig body into `ADeepDelvePlayerController::PerformSwing()`:
```cpp
void ADeepDelvePlayerController::PerformSwing(){
    AActor* R = Econ->CurrentRockActor; if (!IsValid(R)) return;
    float Dmg = GetDamage();
    const bool bCrit = FMath::FRand() < Econ->CritChance;   // FRandomStream if you want reproducibility
    if (bCrit){ Dmg *= Econ->CritMultiplier; if (CritSound) UGameplayStatics::PlaySound2D(this, CritSound); }
    IMineable::Execute_TakeDamage(R, Dmg);
    Econ->ReportDamage(R->GetActorLocation(), Dmg, bCrit);
}
// OnDig just calls PerformSwing();  auto-swing timer also calls PerformSwing();
```
Add `UPROPERTY(EditAnywhere) TObjectPtr<USoundBase> CritSound;` and a `FTimerHandle AutoSwingTimer;` on the controller. In `BeginPlay`, subscribe to `Econ->OnTechChanged` → a `ReconfigureAutoSwing()` that sets/clears the looping timer from `Econ->bAutoSwing` and `Econ->AutoSwingInterval`.
**Update `UMineEconomySubsystem`:** `UPROPERTY() bool bAutoSwing=false; UPROPERTY(EditAnywhere) float AutoSwingInterval=1.f; UPROPERTY(EditAnywhere) float CritChance=0.f; UPROPERTY(EditAnywhere) float CritMultiplier=5.f;` (tunables are `EditAnywhere`).
**New `UTechEffect` subclasses:** `UnlockAutoSwing`, `UpgradeSwingSpeed`, `UnlockCrit`, `UpgradeCritChance` — each writes the matching subsystem field (then `TryBuy` already broadcasts `OnTechChanged`, which retunes the controller's timer).

### 🔵 Blueprints / Data
- `DA_Node_AutoSwing`, `DA_Node_SwingSpeed`, `DA_Node_CritStrike`, `DA_Node_CritChance`. `WBP_DamageNumber` crit variant (gold, bigger — the HUD styles it from the `bCrit` flag). `BP_MineController → CritSound = S_Crit`.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Node_SwingSpeed` | `Effect` (repeatable) | `UpgradeSwingSpeed` (−0.1s/level) |
| `DA_Node_CritChance` | `Effect` (repeatable) | `UpgradeCritChance` (+5%/level) |
| `BP_MineController` | `CritSound` | `S_Crit` |

### Steps
1. Refactor `OnDig` → `PerformSwing()`.
2. Add the auto-swing/crit fields to the subsystem; on the controller add the timer + `ReconfigureAutoSwing()` bound to `OnTechChanged`.
3. Add crit roll + `CritSound` + `bCrit` reporting in `PerformSwing`.
4. New effect subclasses + `DA_Node_*`; build; buy them.

🏁 **Milestone:** hands-free swings + flashy crits.
🧪 **Boss check:** `CritChance = 1.0` → every hit crits.
🎁 **Reward:** Title **Deeplord** · +300 XP.
💡 **Notes & gotchas:** prefer a seeded `FRandomStream` over `FMath::FRand()` if you ever want reproducible runs for testing/balancing.

---

# QUEST 13 — The Growing Crew · ~50 min

**Goal.** Global crew multipliers that update live dwarves.

📚 **Teaches:** applying global modifiers live to many spawned actors via a broadcast event.
**Resources:** [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [Observer pattern](https://gameprogrammingpatterns.com/observer.html) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)

🎮 **After this quest:** buy a bigger, harder-hitting, faster crew; existing dwarves speed up at once.

💡 **Why:** One multiplier scales the whole crew, and broadcasting a change lets dwarves already in the world react (recompute + reset their timer) instead of being destroyed and respawned — the cheap, correct way to update many live actors.

🏛️ **Patterns & principles:** **[Observer](https://refactoring.guru/design-patterns/observer) at scale** — one `OnCrewStatsChanged` broadcast updates N live dwarves without the subsystem holding references to them or looping/polling every frame. This is the idiomatic alternative to `Tick`-based scanning, and it composes with the timer-reset from Q8/Q12.

🔑 **Key nodes / functions:** `float multipliers · DECLARE_DYNAMIC_MULTICAST_DELEGATE (OnCrewStatsChanged) · AddDynamic (in ADwarf::BeginPlay) · GetWorldTimerManager().ClearTimer + SetTimer (reset)`

### 🧩 C++ classes & methods
**New effects:** `UpgradeCrewDamage`, `UpgradeCrewSpeed` → `Econ->CrewDamageMult` / `CrewSpeedMult`.
**Update `UMineEconomySubsystem`:** `UPROPERTY() float CrewSpeedMult=1.f;` + `DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCrewStatsChanged) OnCrewStatsChanged;` broadcast on change.
**Update `ADwarf`:** in `BeginPlay` bind `Econ->OnCrewStatsChanged` → recompute damage and **reset `SwingTimer`** with the new interval (`MinerData->Speed / Econ->CrewSpeedMult`).

### 🔵 Data
- `DA_Node_CrewDamage`, `DA_Node_CrewSpeed`. Optional new `UMinerData` dwarf tiers + hire nodes.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Node_CrewDamage` | `Effect` (repeatable) | `UpgradeCrewDamage` (+0.25/level) |
| `DA_Node_CrewSpeed` | `Effect` (repeatable) | `UpgradeCrewSpeed` (+0.1/level) |

### Steps
1. Add `CrewDamageMult`/`CrewSpeedMult` + `OnCrewStatsChanged` to the subsystem.
2. New effect subclasses; `DA_Node_*`.
3. In `ADwarf::BeginPlay` bind the delegate → recompute + reset the timer.
4. Build; buy Crew Speed with several dwarves; all speed up.

🏁 **Milestone:** upgrades ramp all dwarves live.
🧪 **Boss check:** buy Crew Speed twice with several dwarves; all visibly swing faster.
🎁 **Reward:** Title stays **Deeplord** · +300 XP.

---

# QUEST 14 — Rich Veins · ~55 min

**Goal.** Yield upgrade, new vein tiers, and a lucky mineral drop.

📚 **Teaches:** scaling content through Data Assets + reused effects, and random reward drops.
**Resources:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · [Type Object pattern](https://gameprogrammingpatterns.com/type-object.html) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** richer tiers as you descend, multiplied payouts, and occasional jackpot gems.

💡 **Why:** Because the effect system (Q6) and Data Assets already exist, new tiers + a yield upgrade are pure content — new assets and a reused `Descend` effect, no new systems. The lucky mineral is a rare drop: a jackpot, not steady income (keep the chance low).

🏛️ **Patterns & principles:** this quest is the **payoff of Open/Closed + Type Object** — a whole content expansion (more tiers, a new drop) with *almost no new code*. `AMineral : ACoin` also shows **inheritance for a genuine is-a** (a mineral *is* a collectible coin worth more) as a complement to the composition used for effects.

🔑 **Key nodes / functions:** `float CoinMult · FMath::FRand() (mineral roll) · SpawnActor<ACoin>(MineralClass) · subclass ACoin · new UPrimaryDataAsset tiers · reuse UTechEffect_Descend`

### 🧩 C++ classes & methods
**New effect:** `UTechEffect_UpgradeYield` → `Econ->CoinMult += Amount`.
**`AMineral : ACoin`** (inherits `Collect()`; higher `Value`, gem mesh) — or a `BP_Mineral` subclass of `ACoin`.
**Update `AOreVein`:** apply `Econ->CoinMult` to the coin count; add `UPROPERTY(EditAnywhere) float MineralChance=0.02f; int32 MineralValue=100; TSubclassOf<ACoin> MineralClass;` → on break, roll `MineralChance` → spawn a mineral. Add more `URockData` tiers + `UTechEffect_Descend` nodes.
**Update `UMineEconomySubsystem`:** `UPROPERTY() float CoinMult=1.f;`

### 🔵 Blueprints / Data
- **`BP_Mineral`** (parent `ACoin`). New `DA_` rock tiers (Silver/Gold/Mithril), `DA_Node_Yield`, `DA_Node_Descend_*`.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_Mineral` | `Mesh`/`Value` | Synty gem mesh / `100` |
| `BP_OreVein` | `MineralClass`/`MineralChance` | `BP_Mineral` / `0.02` |

### Steps
1. New effect `UpgradeYield`; add `CoinMult` to the subsystem.
2. `BP_Mineral` (parent `ACoin`); add mineral fields + roll to `AOreVein`; apply `CoinMult` to coins.
3. Author new `DA_` rock tiers + Descend nodes.
4. Build; descend through tiers; confirm jackpots.

🏁 **Milestone:** richer tiers + jackpot gems.
🧪 **Boss check:** `MineralChance = 1.0`, descend a couple tiers, confirm deeper pays more.
🎁 **Reward:** Title stays **Deeplord** · +350 XP.
💡 **Notes & gotchas:** ⚖️ deeper tiers should raise `CoinReward` faster than `MaxHP`; keep `MineralChance` low (jackpot variance, not steady income).

---

# QUEST 15 — The Coin Collector · ~55 min

**Goal.** A walking dwarf that auto-collects coins/minerals by overlap.

📚 **Teaches:** collision/overlap events and per-frame actor movement (Tick + delta time) — and when Tick is the *right* choice.
**Resources:** [Collision & Overlaps](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) · [Actors (ticking)](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) · [Update Method pattern](https://gameprogrammingpatterns.com/update-method.html) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)

🎮 **After this quest:** a dwarf strolls across the mine hoovering up coins on its own.

💡 **Why:** Auto-collecting by touch is a collision/overlap query, not a click — an overlap fires when volumes intersect, exactly what a sweeper needs. The collector moves in `Tick`, scaled by delta time so speed is frame-rate independent. It reuses the coin's existing `Collect()`.

🏛️ **Patterns & principles:**
- **[Update Method](https://gameprogrammingpatterns.com/update-method.html)** — continuous movement genuinely needs per-frame `Tick` (contrast Q8's timer for discrete swings). Scale by `DeltaSeconds` for frame-rate independence — a core real-time game principle.
- **DRY reuse** — the collector calls the *same* public `Collect()` from Q4; collection logic lives in one place.
- **Event-driven collision** — overlaps push notifications (Observer-like) instead of the collector polling every coin.

🔑 **Key nodes / functions:** `USphereComponent · SetGenerateOverlapEvents(true) · OnComponentBeginOverlap.AddDynamic · Cast<ACoin> · Tick / GetDeltaSeconds · AddActorWorldOffset · PrimaryActorTick.bCanEverTick=true`

### 🧩 C++ classes & methods
**`ACollectorDwarf : AActor`**
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<USkeletalMeshComponent> Mesh;
UPROPERTY(VisibleAnywhere) TObjectPtr<class USphereComponent> Reach; // Generate Overlap Events = true
UPROPERTY(EditAnywhere) float Speed = 200.f;
UPROPERTY(EditAnywhere) float LeftX, RightX;
// constructor: PrimaryActorTick.bCanEverTick = true;
virtual void Tick(float Dt) override;   // move left by Speed*Dt*Econ->CollectorSpeedMult; wrap at LeftX
UFUNCTION() void OnReachOverlap(...);   // if (ACoin* C = Cast<ACoin>(Other)) C->Collect();
```
**New effects:** `UTechEffect_HireCollector`, `UTechEffect_UpgradeCollectorSpeed`. **Update subsystem:** `UPROPERTY() float CollectorSpeedMult=1.f;` + spawn collectors. `ACoin::Collect()` is already **public** (Q4).

### 🔵 Blueprints / Data
- **`BP_Collector`** (parent `ACollectorDwarf`), `DA_Node_HireCollector`, `DA_Node_CollectorSpeed`.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_Collector` | `Mesh` / walk anim / `Speed` | Synty dwarf / walk anim / `200` |
| `BP_Collector` | `Reach` | Sphere, **Generate Overlap Events = true**, overlaps coins |

### Steps
1. New PrimaryDataAsset `CollectorData`; New Actor `ACollectorDwarf` (mesh + sphere + Tick + overlap); constructor sets `PrimaryActorTick.bCanEverTick = true`.
2. Tick moves left by `Speed*Dt*CollectorSpeedMult`; wrap at bounds.
3. `OnComponentBeginOverlap` → `Cast<ACoin>` → `Collect()`.
4. New effects + `DA_Node_*`; `BP_Collector`; build.

🏁 **Milestone:** the collector auto-sweeps coins.
🧪 **Boss check:** pile coins; confirm it sweeps them; buy a second collector.
🎁 **Reward:** Title stays **Deeplord** · +300 XP.
💡 **Notes & gotchas:** the constructor must set `PrimaryActorTick.bCanEverTick = true` (or `Tick` never runs); and overlap needs **Generate Overlap Events = true on BOTH** the sphere and the coin, with responses set to overlap.

---

# QUEST 16 — Strike It Rich · ~60 min · *finale*

**Goal.** Juice (animation, Niagara, Sound Cues), an **Object Pool** for coins, and a polished **Shipping** v1.0.

📚 **Teaches:** game "juice" — UMG animation, Niagara VFX, Sound Cues — the Object Pool optimization, and cutting a Shipping release.
**Resources:** [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Object Pool pattern](https://gameprogrammingpatterns.com/object-pool.html) · [UE performance considerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-memory-and-cpu-performance-considerations-in-unreal-engine) · [Packaging](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project)

🎮 **After this quest:** a polished standalone v1.0 — clicks punch, coins ching, crits boom, dust and sparks fly, and hundreds of coins spawn without a hitch.

💡 **Why:** Juice is the feedback that makes actions feel good, cheap to add once systems work. And by now you're spawning/destroying lots of coins — the moment to introduce the [**Object Pool**](https://gameprogrammingpatterns.com/object-pool.html) so those allocations stop churning the GC.

🏛️ **Patterns & principles:**
- **[Object Pool](https://gameprogrammingpatterns.com/object-pool.html)** — keep a pool of inactive `ACoin`s and reuse them (hide/disable on collect, re-activate on spawn) instead of `SpawnActor`/`Destroy` each time. This is the canonical fix for spawn-heavy games; see [UE performance considerations](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-memory-and-cpu-performance-considerations-in-unreal-engine).
- **Polish as a separate layer** — VFX/SFX subscribe to the same events (`OnDamageDealt`, `OnCoinsChanged`) rather than being wired into gameplay, so juice never entangles logic.

🔑 **Key nodes / functions:** `Widget Animation (Designer) · Sound Cue asset · UNiagaraFunctionLibrary::SpawnSystemAtLocation · pooled ACoin (SetActorHiddenInGame/SetActorEnableCollision/reuse) · Build.cs += "Niagara" · Package Project (Shipping)`

### 🧩 C++ classes & methods
- Add `UPROPERTY(EditAnywhere) TObjectPtr<class UNiagaraSystem> BreakVFX;` to `AOreVein` (plus crit sparks, mineral shimmer, coin poof); spawn via `UNiagaraFunctionLibrary::SpawnSystemAtLocation`. Add **`"Niagara"`** to `Build.cs`.
- **Object Pool:** give the subsystem a `UPROPERTY() TArray<TObjectPtr<ACoin>> CoinPool;` — `AcquireCoin()` reuses an inactive coin (or spawns one if the pool is empty) and `Collect()` returns it to the pool (hide + disable) instead of `Destroy()`.
- Optionally convert the raw `PlaySound2D` calls to **Sound Cues** (still `USoundBase` UPROPERTYs — assign a Sound Cue instead of a Sound Wave).

### 🔵 Blueprints
- Assign Niagara systems + Sound Cues on `BP_OreVein`/`BP_Coin`; author UMG animations (DIG punch, purchase pop, unlock flourish); Package Shipping.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_OreVein` | `BreakVFX` | a Niagara dust burst |
| `BP_Coin` | `PickupSound` | `SC_CoinPickup` (Sound Cue) |

### Steps
1. UMG animations; Niagara bursts; ambient/music.
2. Add the coin **Object Pool** to the subsystem; route spawn/collect through it.
3. Set icon/splash/name; **Shipping** config; **Package**; test end-to-end; share.

🏁 **Milestone:** a polished, playable v1.0 outside the editor, smooth even with heavy coin spawns.
🧪 **Boss check:** the Shipping build plays the whole game with sound + effects and no frame hitches when a rich vein bursts.
🎁 **Reward:** **Deeplord, Shaper of the Deep** · +400 XP · 🏅 QUEST LINE COMPLETE.

**🎓 What to learn next:** Gameplay Ability System (GAS), replication & multiplayer, and AI / Behavior Trees.

---

*Engine: Unreal Engine 5.8 · IDE: Rider · Art: Synty POLYGON Dungeon Realms · Docs: dev.epicgames.com. The C++-first chain is fully specified, dependency-audited, and annotated with the architecture patterns and clean-code principles it teaches.*
