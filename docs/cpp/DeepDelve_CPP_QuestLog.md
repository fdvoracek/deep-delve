# 🟢 DEEP DELVE — C++-First Quest Log

> The **C++-first** path: every system is a C++ class from Quest 1, with a thin **Blueprint subclass** that only assigns data (meshes, sounds, Data Assets, input assets) and sets `UPROPERTY` defaults. UMG uses C++ `UUserWidget` base classes with `meta=(BindWidget)` controls and `WBP_` layouts.
>
> **Engine:** Unreal Engine 5.8 · **IDE:** JetBrains Rider · **Art:** Synty POLYGON Dungeon Realms · **API macro used below:** `DEEPDELVE_API` (replace with your module's API macro).
>
> ← Blueprints-first path: https://fdvoracek.github.io/deep-delve/blueprints/

## The working pattern (read once)

For every class we build:
1. **Write the C++ class** — `UPROPERTY` members expose data; specifiers decide who edits them (`EditDefaultsOnly` for class-default assets/tuning, `EditAnywhere` for per-instance, `VisibleAnywhere`/`BlueprintReadOnly` for runtime state, `BlueprintCallable`/`BlueprintNativeEvent` for BP access).
2. **Create a Blueprint subclass** (`BP_…` / `WBP_…`) — assign the values to those `UPROPERTY`s and drop the actor into the level.
3. **Data lives in `UPrimaryDataAsset`s** (`DA_…` instances) referenced by the classes.

Each quest lists: **🎮 After this quest** (the visible result), **🔑 Key nodes / functions** (the exact engine APIs + editor actions to search for), the **C++ classes & methods**, the matching **Blueprints**, a **UPROPERTY → value** table, **Steps**, and Milestone/Boss/Reward.

## The C++-first arc

1. Foundations & First Class — 2. The Player & Enhanced Input — 3. The Rock & the Interface — 4. Coins & the Economy Subsystem — 5. Data-Driven Content — 6. Tech Tree: Model — 7. Tech Tree: UI — 8. Hire the Crew — 9. Menus & Level Flow — 10. Save & Load — 11. Ship a Demo — 12. Pickaxe Upgrades — 13. The Growing Crew — 14. Rich Veins — 15. The Coin Collector — 16. Strike It Rich.

> **Status:** all 16 quests fully specified and **dependency-audited** — nothing is used before it's built.

---

# QUEST 1 — Foundations & First Class · ~60 min

**Goal.** Stand up a **C++** project, wire up Rider, migrate the Synty assets, compose the mine scene, and create your first C++ class — the GameMode — with its Blueprint.

📚 **Teaches:** setting up a C++ Unreal project with Rider, migrating assets between projects, and writing your first class — a Game Mode — with a Blueprint subclass.
**Resources:** [Game Mode & Game State](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) · [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Gameplay Classes (C++/BP)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine) · [C++ Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start) · [Migrating Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/migrating-assets-in-unreal-engine) · *Further:* [Your First Hour in Unreal Engine](https://dev.epicgames.com/community/learning/courses/3ke/your-first-hour-in-unreal-engine-5-2)

🎮 **After this quest:** the project compiles from Rider, your mine scene renders through a fixed camera, and your own `ADeepDelveGameMode` (via `BP_DeepDelveGameMode`) is the active game mode.

💡 **Why:** A [**Game Mode**](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) is Unreal's rulebook for a play session — it decides *which* PlayerController, Pawn, HUD and GameState classes are spawned and owns session-level rules. Even a click game needs one so the engine spawns *our* controller (Q2) instead of the default. We extend **`AGameModeBase`** (the lightweight base) rather than `AGameMode` because Deep Delve is single-player and doesn't need `AGameMode`'s multiplayer match-state machinery. We write it in **C++ with a Blueprint subclass** — the [standard Unreal workflow](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine): logic in C++, class/asset choices in the BP so you change them without recompiling. (Overview: [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine).)

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

💡 **Why:** The [**PlayerController**](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) represents the human player's *will* — it receives input and turns it into commands, and it persists independently of any pawn. That makes it the natural home for player-owned, non-visual state like `Damage` and the input bindings, so we subclass it. We use **[Enhanced Input](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine)** (UE5's current input system, which replaces the legacy action/axis mappings) because it's data-driven, remappable, and the recommended approach: a left-click becomes a reusable `IA_Dig` Input Action instead of a hard-coded key.

🔑 **Key nodes / functions:** `New C++ Class (PlayerController) · Build.cs: PublicDependencyModuleNames += "EnhancedInput" · ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem> · AddMappingContext · UEnhancedInputComponent::BindAction · SetInputMode(FInputModeGameAndUI) · bShowMouseCursor · Input Action / Input Mapping Context assets`

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
Add `"EnhancedInput"` to `PublicDependencyModuleNames` in `DeepDelve.Build.cs`.

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

📚 **Teaches:** C++ interfaces, Actors, decoupled damage through one shared interface, and playing sounds.
**Resources:** [Actors](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) · [Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [Audio Engine Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/audio-engine-overview-in-unreal-engine) · *Further:* [Actor Lifecycle](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-actor-lifecycle)

🎮 **After this quest:** clicking the rock damages it via the interface; at 0 HP it breaks (with sound) and a fresh rock spawns.

💡 **Why:** The rock is a placeable, spawnable, tickable thing in the level — which is exactly what an [**Actor**](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) is (a container of components with a transform). The **[interface `IMineable`](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine)** is the key idea: it lets the *caller* deal damage without knowing the *callee's* concrete type. Anything implementing `IMineable` can be mined, so your clicks now — and dwarves and auto-swing later — all go through one `TakeDamage` call with zero extra coupling, and new mineable types need no change to the caller. `BlueprintNativeEvent` lets C++ define the default behavior while Blueprints can still respond.

🔑 **Key nodes / functions:** `New C++ Class→Unreal Interface · UINTERFACE(BlueprintType) · UFUNCTION(BlueprintNativeEvent) · TakeDamage_Implementation · GetHitResultUnderCursor(ECC_Visibility) · AActor::Implements<UMineable>() · IMineable::Execute_TakeDamage · UGameplayStatics::PlaySound2D · SpawnActor / Destroy`

### 🧩 C++ classes & methods
**Interface `UMineable`/`IMineable`:**
```cpp
UINTERFACE(BlueprintType) class UMineable : public UInterface { GENERATED_BODY() };
class IMineable { GENERATED_BODY()
public: UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category="Mining") void TakeDamage(float Amount); };
```
**`AOreVein : AActor, public IMineable`:**
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
💡 **Notes & gotchas:** the rock needs collision the cursor trace can hit (default Synty mesh collision blocking Visibility is fine).

---

# QUEST 4 — Coins & the Economy Subsystem · ~60 min

**Goal.** A **`UGameInstanceSubsystem`** for the economy, a collectible **`ACoin`**, a C++ **HUD widget** with `BindWidget`, and one shared floating-number helper.

📚 **Teaches:** GameInstance Subsystems, event dispatchers, collectible Actors, and C++ UMG widgets with `BindWidget`.
**Resources:** [Programming Subsystems](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) · [Gameplay Framework (GameInstance)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine)

🎮 **After this quest:** broken rocks burst coins you sweep up (with a "ching"); a HUD shows Coins + Depth; a "1" floats off each hit.

💡 **Why:** Coins and Depth must survive moving between the menu and mine levels, so they can't live on an Actor or the GameMode (recreated per level) — they belong to something with the **GameInstance's** whole-game lifetime. A [**GameInstance Subsystem**](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-subsystems-in-unreal-engine) is an auto-created, globally-reachable module attached to that lifetime — cleaner than subclassing the GameInstance. We use an [**Event Dispatcher**](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) (`OnCoinsChanged`) so the HUD refreshes only when coins change (push) rather than polling every frame. The coin is an [Actor](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) (a physical, collectible world object), and the HUD uses a C++ [**UMG**](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) `UUserWidget` base with `BindWidget` so code has type-safe access while a `WBP_` owns the layout.

🔑 **Key nodes / functions:** `UGameInstanceSubsystem · GetGameInstance()->GetSubsystem<UMineEconomySubsystem>() · DECLARE_DYNAMIC_MULTICAST_DELEGATE + Broadcast · UBlueprintFunctionLibrary · SimulatePhysics/AddImpulse · NotifyActorBeginCursorOver · bEnableMouseOverEvents · CreateWidget/AddToViewport · meta=(BindWidget) · ProjectWorldLocationToScreen · SetLifeSpan`

### 🧩 C++ classes & methods
**`UMineEconomySubsystem : UGameInstanceSubsystem`** (no Blueprint — auto-instantiated):
```cpp
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCoinsChanged);
UPROPERTY(BlueprintReadOnly) double Coins = 0, Depth = 0;
UPROPERTY(BlueprintAssignable) FOnCoinsChanged OnCoinsChanged;
UFUNCTION(BlueprintCallable) void AddCoins(double A);  // Coins+=A; OnCoinsChanged.Broadcast();
UFUNCTION(BlueprintCallable) void AddDepth(double A);
```
**`UMineStatics : UBlueprintFunctionLibrary`** — `UFUNCTION(BlueprintPure) static FText FormatNumber(double Value);` (1500→"1.5K").
**`ACoin : AActor`** — `UStaticMeshComponent* Mesh` (Simulate Physics in BP); `int32 Value=1; float LaunchImpulse=300; float Lifespan=30; USoundBase* PickupSound;` `BeginPlay` (random `AddImpulse`, `SetLifeSpan`); `NotifyActorBeginCursorOver` → `Collect()`; **`void Collect();`** (subsystem `AddCoins(Value)`; `PlaySound2D`; `Destroy`) — **keep `Collect()` public** (the collector reuses it in Q15).
**`UHUDWidget : UUserWidget`** — `UPROPERTY(meta=(BindWidget)) UTextBlock* CoinsText, *DepthText;` `NativeConstruct` binds `OnCoinsChanged`→`Refresh`; `Refresh()` sets texts via `UMineStatics::FormatNumber`.
**Update `AOreVein`** — `int32 CoinReward=10; TSubclassOf<ACoin> CoinClass;` break branch loops `CoinReward`→`SpawnActor<ACoin>`, `Econ->AddDepth(1)`.
**Update `ADeepDelvePlayerController`** — in `BeginPlay` `bEnableMouseOverEvents=true` + create the HUD (`TSubclassOf<UHUDWidget> HUDClass`). Add the **shared floating-number helper** (used by clicks now, dwarves Q8, crits Q12):
```cpp
UPROPERTY(EditDefaultsOnly) TSubclassOf<UUserWidget> DamageNumberClass;
UFUNCTION(BlueprintCallable) void ShowDamageNumber(FVector WorldLocation, float Amount, bool bCrit=false);
// ProjectWorldLocationToScreen(...); CreateWidget(DamageNumberClass); set text/colour; add to viewport; anim floats+fades+RemoveFromParent.
```
`OnDig` calls `ShowDamageNumber(Hit.ImpactPoint, Amount)` **after** dealing damage (the rock never spawns numbers, so crits/dwarves style their own).

> **Build.cs:** the first `UUserWidget` (`UHUDWidget`) means adding **`"UMG"`, `"Slate"`, `"SlateCore"`** to `PublicDependencyModuleNames` — or nothing UMG-related compiles.

### 🔵 Blueprints to create
- **`BP_Coin`** (parent `ACoin`), **`WBP_HUD`** (parent `UHUDWidget`), **`WBP_DamageNumber`** (a `UUserWidget` with a float-up+fade animation).

### 🎛️ UPROPERTY → values
| Class | UPROPERTY | Assign |
|---|---|---|
| `BP_Coin` | `Mesh` | Synty coin mesh; **Simulate Physics = true** |
| `BP_Coin` | `Value` / `PickupSound` | `1` / `S_CoinPickup` |
| `WBP_HUD` | `CoinsText`, `DepthText` | Text Blocks named exactly `CoinsText`/`DepthText` |
| `BP_MineController` | `HUDClass` | `WBP_HUD` |
| `BP_MineController` | `DamageNumberClass` | `WBP_DamageNumber` |
| `BP_OreVein` | `CoinReward` / `CoinClass` | `10` / `BP_Coin` |

### Steps
1. Create the subsystem, `UMineStatics`, `ACoin`, `UHUDWidget`; add `UMG`/`Slate`/`SlateCore` to `Build.cs`; build.
2. Update `AOreVein` (coin fields + spawn loop) and the controller (mouse-over + HUD + `ShowDamageNumber`).
3. Create `BP_Coin`, `WBP_HUD` (match `BindWidget` names), `WBP_DamageNumber`; assign the table.
4. Play; break rocks; collect coins.

🏁 **Milestone:** break → coins burst → hover to collect → HUD climbs, Depth ticks, damage numbers float.
🧪 **Boss check:** a coin credits exactly once and the HUD total matches.
🎁 **Reward:** Title **Miner** · +200 XP.
💡 **Notes & gotchas:** `BindWidget` requires the `WBP_HUD` widget names to **exactly match** the C++ members. Uncollected coins auto-clean via `SetLifeSpan`.

---

# QUEST 5 — Data-Driven Content · ~50 min

**Goal.** Move rock and pickaxe stats into **`UPrimaryDataAsset`s**, and route the active rock/pickaxe through the subsystem so Descend (Q6) can swap tiers.

📚 **Teaches:** data-driven design with Data Assets (`UPrimaryDataAsset`) so content is authored, not coded.
**Resources:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · *Further:* [Data-Driven Gameplay Elements](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-driven-gameplay-elements-in-unreal-engine)

🎮 **After this quest:** identical to play, but a new rock/pickaxe is a new `DA_` asset — no recompile.

💡 **Why:** Hard-coding a rock's mesh/HP in the class means every new rock is a code change. [**Data Assets**](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) (`UPrimaryDataAsset`) separate *data* from *behavior*, so new rocks and pickaxes are just assets you author in the editor — no recompile, no new classes. Routing the active rock/pickaxe through the subsystem gives one source of truth that the tech tree (Q6) can change at runtime.

🔑 **Key nodes / functions:** `UPrimaryDataAsset · right-click→Miscellaneous→Data Asset · TObjectPtr<> asset references · UStaticMeshComponent::SetStaticMesh · GetGameInstance()->GetSubsystem<>()`

### 🧩 C++ classes & methods
**`URockData : UPrimaryDataAsset`** — `UStaticMesh* Mesh; int32 MaxHP=10; int32 CoinReward=10;`
**`UPickaxeData : UPrimaryDataAsset`** — `UStaticMesh* Mesh; float Damage=1; float Speed=1;`
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
**Add to `UMineEconomySubsystem`:** `TObjectPtr<URockData> CurrentRock; TObjectPtr<AActor> CurrentRockActor; TSubclassOf<AOreVein> OreVeinClass; FTransform LastRockTransform; TObjectPtr<UPickaxeData> EquippedPickaxe;`
**Update `ADeepDelvePlayerController`:** `UPROPERTY(EditDefaultsOnly) TObjectPtr<UPickaxeData> DefaultPickaxe;` + `float GetDamage() const;` (= `Econ->EquippedPickaxe->Damage`; Q6 adds `+ PickaxeDamageBonus`). In `BeginPlay`: `if(!Econ->EquippedPickaxe) Econ->EquippedPickaxe=DefaultPickaxe;` `OnDig` sends `GetDamage()`.

### 🔵 Data Assets & Blueprint values
- `DA_CopperVein` (`URockData`: 10/10), `DA_IronVein` (25/30), `DA_StonePick` (`UPickaxeData`: Damage 1), `DA_SturdyPick` (Damage 3).

| Class | UPROPERTY | Assign |
|---|---|---|
| `BP_OreVein` | `RockData` | `DA_CopperVein` |
| `BP_MineController` | `DefaultPickaxe` | `DA_StonePick` |

### Steps
1. Create `URockData`, `UPickaxeData`; build; make the four `DA_` instances.
2. Add the subsystem fields; update `AOreVein::BeginPlay` and the controller's damage source.
3. Assign `RockData`/`DefaultPickaxe` in the BPs.
4. Prove it: point `RockData` at `DA_IronVein` → tougher/richer with no recompile.

🏁 **Milestone:** a new rock type + pickaxe added purely as `DA_` assets.
🧪 **Boss check:** create a third `URockData` in under two minutes and slot it in.
🎁 **Reward:** Title stays **Miner** · +200 XP.

---

# QUEST 6 — Tech Tree: the Model · ~65 min

**Goal.** A data-driven tech tree with a **polymorphic `UTechEffect`** hierarchy driving every upgrade; all modifiers live on the economy subsystem.

📚 **Teaches:** a polymorphic `UObject` effect system (virtual `Apply`) and a data-driven tech-tree model.
**Resources:** [Objects (UObject)](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects-in-unreal-engine) · [Gameplay Classes](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-classes-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** buy the first three nodes (temporary button; UI next quest): Sharpen → Iron Pickaxe → Descend, and descending actually swaps the rock.

💡 **Why:** Every upgrade *does something different*, but the tree shouldn't care what. Giving each node its own [**`UObject`**](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects-in-unreal-engine) effect with a virtual `Apply()` means adding an upgrade is *adding a small class*, not editing an ever-growing `switch` (the open/closed principle). Effects are `UObject`s, not Actors, because they're lightweight logic with no presence in the world; `Instanced` lets each node own its configured effect. State lives on the subsystem so effects have one place to mutate and progress persists. (Design background: [incremental games](https://en.wikipedia.org/wiki/Incremental_game).)

🔑 **Key nodes / functions:** `UCLASS(Abstract, EditInlineNew, DefaultToInstanced) · virtual Apply() override · UPROPERTY(Instanced) · TMap<FName,int32> · FMath::Pow · TArray Prerequisites · UGameplayStatics::GetActorOfClass(ACameraActor) · AddActorWorldOffset · SpawnActor<AOreVein>`

### 🧩 C++ classes & methods
**`UTechEffect : UObject`** (base):
```cpp
UCLASS(Abstract, EditInlineNew, DefaultToInstanced, BlueprintType)
class DEEPDELVE_API UTechEffect : public UObject { GENERATED_BODY()
public: UFUNCTION() virtual void Apply(class UMineEconomySubsystem* Econ, int32 NewLevel) {} };
```
**Subclasses:** `UTechEffect_UpgradePickaxeDamage` (`float AmountPerLevel=1;` → `Econ->PickaxeDamageBonus += AmountPerLevel`); `UTechEffect_EquipPickaxe` (`UPickaxeData* Pickaxe;` → `Econ->EquippedPickaxe = Pickaxe`); `UTechEffect_Descend` (`URockData* NextRock;` → `Econ->SetCurrentRock(NextRock)`).
**`UTechNode : UPrimaryDataAsset`** — `FName NodeId; FText DisplayName, Description; UTexture2D* Icon; int32 Cost=10; float CostGrowth=1.15f; bool bRepeatable=false; int32 MaxLevel=1; TArray<UTechNode*> Prerequisites; FVector2D GridPosition; UPROPERTY(Instanced) UTechEffect* Effect;`
**Update `UMineEconomySubsystem`** (`EquippedPickaxe`/`CurrentRock`/`CurrentRockActor`/`OreVeinClass`/`LastRockTransform` exist from Q5):
```cpp
UPROPERTY() TMap<FName,int32> NodeLevels;
UPROPERTY(BlueprintReadOnly) float PickaxeDamageBonus = 0.f;
UPROPERTY(EditAnywhere) float DescendCameraStep = 150.f;
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnTechChanged); UPROPERTY(BlueprintAssignable) FOnTechChanged OnTechChanged;
UFUNCTION(BlueprintCallable) int32 GetCost(UTechNode* N) const;     // Cost * CostGrowth^level
UFUNCTION(BlueprintCallable) bool  CanBuy(UTechNode* N) const;
UFUNCTION(BlueprintCallable) bool  TryBuy(UTechNode* N);            // spend, level++, Effect->Apply, Broadcast
UFUNCTION() void SetCurrentRock(URockData* NewRock);
// SetCurrentRock: CurrentRock=NewRock; if(CurrentRockActor)CurrentRockActor->Destroy();
//   GetWorld()->SpawnActor<AOreVein>(OreVeinClass, LastRockTransform);       // new rock reads CurrentRock
//   if(AActor* Cam=UGameplayStatics::GetActorOfClass(this,ACameraActor::StaticClass())) Cam->AddActorWorldOffset({0,0,-DescendCameraStep});
```
`GetDamage()` now returns `EquippedPickaxe->Damage + PickaxeDamageBonus`.

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
🧪 **Boss check:** add a 4th node as a `DA_` + one new effect subclass.
🎁 **Reward:** Title **Pit Foreman** · +300 XP.
💡 **Notes & gotchas:** mark `UTechEffect` `EditInlineNew`+`DefaultToInstanced` and the node's `Effect` `Instanced`, or you can't create effect objects inline on the Data Asset. ⚖️ `CostGrowth` (~1.10–1.15) sets the whole game's pacing — tune after the demo.

---

# QUEST 7 — Tech Tree: the UI · ~60 min

**Goal.** The real tech-tree screen from C++ `UUserWidget` bases + `WBP_` layouts.

📚 **Teaches:** building data-driven UMG screens in C++ — dynamic widgets on a Canvas with custom-drawn connectors.
**Resources:** [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) · [Widget Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) · *Further:* [UMG UI Designer Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/umg-ui-designer-quick-start-guide-in-unreal-engine)

🎮 **After this quest:** a screen of connected nodes that light up when affordable and check off when bought.

💡 **Why:** Building the screen *from the node data* means the UI reflects the tree automatically — add a node asset and it appears, with no layout rework. A [**UMG**](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine) Canvas gives free placement from each node's `GridPosition`; the C++ widget base holds the logic (buy, refresh, connectors) while the [`WBP_`](https://dev.epicgames.com/documentation/en-us/unreal-engine/widget-blueprints-in-umg-for-unreal-engine) supplies the look — the same code/design split as the HUD.

🔑 **Key nodes / functions:** `UUserWidget · meta=(BindWidget) UButton/UImage/UTextBlock · UCanvasPanel · CreateWidget (in a loop) · UCanvasPanelSlot::SetPosition · NativePaint → UWidgetBlueprintLibrary::DrawLine · Button->OnClicked.AddDynamic · SetIsEnabled / SetColorAndOpacity · GetSubsystem OnTechChanged`

### 🧩 C++ classes & methods
**`UTechNodeWidget : UUserWidget`** — `meta=(BindWidget) UButton* BuyButton; UImage* IconImage; UTextBlock* NameText,*CostText;` `UTechNode* Node;` `void Setup(UTechNode*)` (fill icon/name; `BuyButton->OnClicked.AddDynamic(this,&…::OnBuyClicked)`); `UFUNCTION() void OnBuyClicked()` (`Econ->TryBuy(Node)`); `UFUNCTION() void Refresh()` (colour by `CanBuy`/owned/maxed; `SetIsEnabled`).
**`UTechTreeWidget : UUserWidget`** — `meta=(BindWidget) UCanvasPanel* Canvas;` `EditDefaultsOnly TSubclassOf<UTechNodeWidget> NodeWidgetClass; TArray<UTechNode*> AllNodes;` `NativeConstruct` (spawn a node widget per `AllNodes` at `GridPosition`; bind `OnTechChanged`→RefreshAll); `NativePaint` (draw connector lines from each node's Prerequisites).

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
**Resources:** [Skeletal Mesh Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-sequences-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Blueprint Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-interface-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · *Further:* [Anim Notifies](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine)

🎮 **After this quest:** buy "Hire Dwarf" → a dwarf appears and mines the rock on his own.

💡 **Why:** A dwarf is a visible, animated, spawnable world entity → an [**Actor**](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine). He mines through the **same `IMineable` interface** your clicks use — the payoff of Q3: idle income needs *no* new coupling to the rock. We drive his swing with a looping [**Timer**](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) rather than `Tick` because a swing happens on a fixed cadence, not every frame — cheaper and simpler.

🔑 **Key nodes / functions:** `USkeletalMeshComponent · Anim: SetAnimationMode(AnimationSingleNode)/PlayAnimation (or an Anim asset) · GetWorld()->GetTimerManager().SetTimer(handle, this, &…::Swing, Interval, true) · IMineable::Execute_TakeDamage · GetGameInstance()->GetFirstLocalPlayerController()->ShowDamageNumber · SpawnActor`

### 🧩 C++ classes & methods
**`UMinerData : UPrimaryDataAsset`** — `USkeletalMesh* Mesh; UAnimationAsset* MineAnim; float Damage; float Speed;`
**`ADwarf : AActor`** — `USkeletalMeshComponent* Mesh; UMinerData* MinerData; USoundBase* HitSound; FTimerHandle SwingTimer;` `BeginPlay` (set mesh + play `MineAnim`; start looping timer at `MinerData->Speed`); `void Swing()` (`Econ->CurrentRockActor` → `IMineable::Execute_TakeDamage(rock, MinerData->Damage * Econ->CrewDamageMult)`; `PlaySound2D(HitSound)`; call the controller's `ShowDamageNumber` at the rock).
**`UTechEffect_HireMiner : UTechEffect`** — `UMinerData* Miner; TSubclassOf<ADwarf> DwarfClass;` → `Econ->SpawnDwarf(DwarfClass, Miner)`.
**Update `UMineEconomySubsystem`** — `float CrewDamageMult=1.f; TArray<UMinerData*> HiredMiners; TSubclassOf<ADwarf> DwarfClass;` and `void SpawnDwarf(TSubclassOf<ADwarf> Class, UMinerData* Miner)` (spawn at a ring slot; record `DwarfClass=Class; HiredMiners.Add(Miner);` for Q10). (`CurrentRockActor` exists from Q5.)

### 🔵 Blueprints / Data
- **`BP_Dwarf`** (parent `ADwarf`), **`DA_Dwarf1`** (`UMinerData`), **`DA_Node_HireDwarf`** (`UTechNode`, repeatable).

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Dwarf1` | `Mesh`/`MineAnim`/`Damage`/`Speed` | Synty dwarf mesh / mine anim / `1` / `1.0` |
| `BP_Dwarf` | `MinerData`/`HitSound` | `DA_Dwarf1` / `S_RockHit` |
| `DA_Node_HireDwarf` | `Effect` | `HireMiner(Miner=DA_Dwarf1, DwarfClass=BP_Dwarf)` |

### Steps
1. New PrimaryDataAsset `MinerData`; New Actor `ADwarf` (mesh + timer + `Swing`).
2. New `UTechEffect_HireMiner`; extend the subsystem (`CrewDamageMult`, `HiredMiners`, `DwarfClass`, `SpawnDwarf`).
3. Create `BP_Dwarf`, `DA_Dwarf1`, `DA_Node_HireDwarf`; assign values.
4. Build; buy Hire Dwarf; watch it mine.

🏁 **Milestone:** a hired dwarf mines hands-free.
🧪 **Boss check:** hire two; both damage the rock independently.
🎁 **Reward:** Title **Master Smith** · +300 XP.
💡 **Notes & gotchas:** ⭐ optional — deal damage on the exact swing frame with an **Anim Notify** instead of the timer.

---

# QUEST 9 — Menus & Level Flow · ~55 min

**Goal.** A Main Menu level and an Esc pause menu, driven by a C++ `UMenuWidget`.

📚 **Teaches:** level management (Open Level), UMG menus, input modes, and pausing.
**Resources:** [Gameplay Framework](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) · [Game Mode & Game State](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) · [UMG](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-user-interfaces-with-umg-and-slate-in-unreal-engine)

🎮 **After this quest:** the game boots to a title screen (New Game / Quit); Esc pauses in the mine.

💡 **Why:** Splitting the title screen and the game into separate **levels** keeps their concerns apart and is how Unreal organizes scenes; `OpenLevel` swaps them while the [GameInstance](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine) (and our economy subsystem) persists across the change — which is exactly *why* the economy lives there. The menu gets its own [Game Mode](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-mode-and-game-state-in-unreal-engine) so the mining controller/HUD never run on the title screen.

🔑 **Key nodes / functions:** `UGameplayStatics::OpenLevel(FName) · UKismetSystemLibrary::QuitGame · SetInputMode(FInputModeUIOnly) · SetPause / Set Game Paused · Level Blueprint Event BeginPlay · World Settings→GameMode Override · Project Settings→Maps & Modes (Game Default Map) · Input Action IA_Pause`

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

🎮 **After this quest:** quit, relaunch, Continue → everything is back.

💡 **Why:** [**`USaveGame`**](https://dev.epicgames.com/documentation/en-us/unreal-engine/saving-and-loading-your-game-in-unreal-engine) is Unreal's built-in container for persistent progress, serialized to a named slot on disk — no custom file I/O. We store asset references as **soft pointers** so the save keeps a stable path and resolves it on load without any Asset Manager setup. And loading *rebuilds the world*, because saving data isn't the same as recreating actors — the dwarves and current rock must be respawned from the saved state.

🔑 **Key nodes / functions:** `USaveGame · UGameplayStatics::CreateSaveGameObject · SaveGameToSlot · DoesSaveGameExist · LoadGameFromSlot · Cast<> · DeleteGameInSlot · TSoftObjectPtr::LoadSynchronous · FDateTime::UtcNow · looping autosave timer`

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
💡 **Notes & gotchas:** bump `SaveVersion` and default missing fields on load — Q13–16 add fields and old saves must not crash.

---

# QUEST 11 — Ship a Demo · ~45 min

**Goal.** Package the C++ build and send it to a friend.

📚 **Teaches:** packaging and distributing a standalone build.
**Resources:** [Packaging Your Project](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) · *Further:* [Packaging & Shipping](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-and-shipping-in-unreal-engine)

🎮 **After this quest:** a standalone Deep Delve demo a friend can run with no Unreal installed.

💡 **Why:** Shipping early is a feature, not a nicety — it gives real feedback, keeps motivation high, and proves the whole loop runs *outside* the editor. [**Packaging**](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) produces a self-contained build the player runs with nothing installed; doing it now (before more systems) keeps any packaging problems small and easy to isolate.

🔑 **Key nodes / functions:** `Platforms→Windows→Package Project · Build Configuration: Development · Project Settings→Maps & Modes · Project Settings→Description (name/icon) · run the packaged .exe`

### 🧩 C++ work
- None new. (A C++ project needs the toolchain to package — but the packaged build is self-contained; recipients need nothing installed.)

### Steps
1. Confirm Game Default Map = MainMenu, project name/icon set.
2. **Platforms → Windows → Package Project**, config **Development**.
3. Test the `.exe`: new game → upgrade → save → quit → relaunch → Continue.
4. Zip + share (Drive/WeTransfer).

🏁 **Milestone:** a shippable demo. 🧪 **Boss check:** run it on a clean folder/machine, full cycle. 🎁 **Reward:** Title stays **Master Smith** · +250 XP · 🎉
💡 **Notes & gotchas:** tune the economy (`Cost`/`CostGrowth` on the tech nodes) from your friend's feedback before adding content.

---

# QUEST 12 — Pickaxe Upgrades: Auto-Swing & Crits · ~55 min

**Goal.** New `UTechEffect` subclasses that add an auto-swing and critical hits, sharing one swing path.

📚 **Teaches:** extending a system with new effect classes; randomness (crits), timers, and a single shared action path (DRY).
**Resources:** [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Programming with C++](https://dev.epicgames.com/documentation/en-us/unreal-engine/programming-with-cplusplus-in-unreal-engine) · [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** the pickaxe swings on its own and occasionally lands a gold "CRIT!".

💡 **Why:** Manual and automatic swings must apply damage, roll crits, play sounds and show numbers *identically* — so we funnel both through one `PerformSwing()` instead of duplicating the logic (DRY). Random crits add the moment-to-moment variance that keeps clicking satisfying, and the auto-swing uses a looping [Timer](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) for the same fixed-cadence reason as the dwarves.

🔑 **Key nodes / functions:** `GetWorld()->GetTimerManager().SetTimer (auto-swing, looping) · FMath::FRand() · GetGameInstance()->GetFirstLocalPlayerController() · Widget Animation · UGameplayStatics::PlaySound2D`

### 🧩 C++ classes & methods
**One swing path.** Move the manual-dig body into **`ADeepDelvePlayerController::PerformSwing()`** — deal `GetDamage()` to `Econ->CurrentRockActor` via `IMineable`, roll `FMath::FRand() < Econ->CritChance` → ×`Econ->CritMultiplier`, play `CritSound` (`UPROPERTY TObjectPtr<USoundBase> CritSound`), and call `ShowDamageNumber(loc, dmg, bCrit)`. `OnDig` just calls `PerformSwing()`.
**Update `UMineEconomySubsystem`:** `bool bAutoSwing; float AutoSwingInterval=1.f; float CritChance=0; float CritMultiplier=5.f;` + an auto-swing looping timer that calls the controller's `PerformSwing()` (via `GetGameInstance()->GetFirstLocalPlayerController()`) — manual and auto share the crit + damage-number logic; **no UI code in the subsystem**.
**New `UTechEffect` subclasses:** `UnlockAutoSwing`, `UpgradeSwingSpeed`, `UnlockCrit`, `UpgradeCritChance` (each writes a subsystem field).

### 🔵 Blueprints / Data
- `DA_Node_AutoSwing`, `DA_Node_SwingSpeed`, `DA_Node_CritStrike`, `DA_Node_CritChance`. `WBP_DamageNumber` crit variant (gold, bigger).

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Node_SwingSpeed` | `Effect` (repeatable) | `UpgradeSwingSpeed` (−0.1s/level) |
| `DA_Node_CritChance` | `Effect` (repeatable) | `UpgradeCritChance` (+5%/level) |
| `BP_MineController` | `CritSound` | `S_Crit` |

### Steps
1. Refactor `OnDig` → `PerformSwing()`.
2. Add auto-swing + crit fields + the auto-swing timer (calls `PerformSwing`) to the subsystem.
3. Add crit roll + `CritSound` + crit-styled number in `PerformSwing`.
4. New effect subclasses + `DA_Node_*`; build; buy them.

🏁 **Milestone:** hands-free swings + flashy crits.
🧪 **Boss check:** `CritChance = 1.0` → every hit crits.
🎁 **Reward:** Title **Deeplord** · +300 XP.

---

# QUEST 13 — The Growing Crew · ~50 min

**Goal.** Global crew multipliers that update live dwarves.

📚 **Teaches:** applying global modifiers live to many spawned actors via a broadcast event.
**Resources:** [Event Dispatchers](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) · [Gameplay Timers](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-timers-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)

🎮 **After this quest:** buy a bigger, harder-hitting, faster crew; existing dwarves speed up at once.

💡 **Why:** One global multiplier scales the *entire* crew at once, and broadcasting an [**event**](https://dev.epicgames.com/documentation/en-us/unreal-engine/event-dispatchers-in-unreal-engine) when it changes lets dwarves already in the world react (recompute + reset their timer) instead of being destroyed and respawned — the correct, cheap way to update many live actors.

🔑 **Key nodes / functions:** `float multipliers · DECLARE_DYNAMIC_MULTICAST_DELEGATE (OnCrewStatsChanged) · Delegate.AddDynamic (in ADwarf::BeginPlay) · GetWorldTimerManager().ClearTimer + SetTimer (reset) · iterate spawned dwarves`

### 🧩 C++ classes & methods
**New effects:** `UpgradeCrewDamage`, `UpgradeCrewSpeed` → `Econ->CrewDamageMult` / `CrewSpeedMult`.
**Update `UMineEconomySubsystem`:** `float CrewSpeedMult=1.f;` + `DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCrewStatsChanged) OnCrewStatsChanged;` broadcast on change.
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
**Resources:** [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) · *Further:* [Incremental-game design](https://en.wikipedia.org/wiki/Incremental_game)

🎮 **After this quest:** richer tiers as you descend, multiplied payouts, and occasional jackpot gems.

💡 **Why:** Because the effect system (Q6) and [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine) already exist, new vein tiers and a yield upgrade are *pure content* — new assets plus a reused `Descend` effect, no new systems. The lucky mineral is a rare random drop: an occasional jackpot that adds excitement without becoming steady income (keep the chance low).

🔑 **Key nodes / functions:** `float CoinMult · FMath::FRand() (mineral roll) · SpawnActor<ACoin>(MineralClass) · subclass ACoin · new UPrimaryDataAsset rock tiers · reuse UTechEffect_Descend`

### 🧩 C++ classes & methods
**New effect:** `UTechEffect_UpgradeYield` → `Econ->CoinMult += Amount`.
**`AMineral : ACoin`** (inherits `Collect()`; higher `Value`, gem mesh) — or a `BP_Mineral` subclass of `ACoin`.
**Update `AOreVein`:** apply `Econ->CoinMult` to the coin count; add `float MineralChance=0.02f; int32 MineralValue=100; TSubclassOf<ACoin> MineralClass;` → on break, roll `MineralChance` → spawn a mineral. Add more `URockData` tiers + `UTechEffect_Descend` nodes.
**Update `UMineEconomySubsystem`:** `float CoinMult=1.f;`

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

📚 **Teaches:** collision/overlap events and per-frame actor movement (Tick + delta time).
**Resources:** [Collision Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) · [Actors (ticking)](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine) · [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine)

🎮 **After this quest:** a dwarf strolls across the mine hoovering up coins on its own.

💡 **Why:** Auto-collecting by *touch* is a [**collision/overlap**](https://dev.epicgames.com/documentation/en-us/unreal-engine/collision-in-unreal-engine---overview) query, not a click — an overlap event fires when two volumes intersect, exactly what a moving sweeper needs. The collector moves every frame in `Tick`, scaled by *delta time* so its speed is frame-rate independent ([Actor ticking](https://dev.epicgames.com/documentation/en-us/unreal-engine/actors-in-unreal-engine)). It calls the coin's existing `Collect()`, reusing Q4's logic rather than duplicating it.

🔑 **Key nodes / functions:** `USphereComponent · SetGenerateOverlapEvents(true) · OnComponentBeginOverlap.AddDynamic · Cast<ACoin> · AActor::Tick / GetWorld()->GetDeltaSeconds · AddActorWorldOffset · PrimaryActorTick.bCanEverTick`

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
**New effects:** `UTechEffect_HireCollector`, `UTechEffect_UpgradeCollectorSpeed`. **Update subsystem:** `float CollectorSpeedMult=1.f;` + spawn collectors. `ACoin::Collect()` is already **public** (Q4).

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
💡 **Notes & gotchas:** the constructor must set `PrimaryActorTick.bCanEverTick = true` (or `Tick` never runs); overlap needs **Generate Overlap Events = true on BOTH** the sphere and the coin.

---

# QUEST 16 — Strike It Rich · ~60 min · *finale*

**Goal.** Juice (animation, Niagara, Sound Cues) + a polished **Shipping** v1.0.

📚 **Teaches:** game "juice" — UMG animation, Niagara VFX, Sound Cues — and cutting a Shipping release.
**Resources:** [UMG Animation](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) · [Sound Cues](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) · [Niagara VFX](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) · [Packaging (Shipping)](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project)

🎮 **After this quest:** a polished standalone v1.0 — clicks punch, coins ching, crits boom, dust and sparks fly.

💡 **Why:** "Juice" — animation, sound and particles — is the feedback that makes actions *feel* good, and it's cheap to add once the systems work. [**Niagara**](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-in-niagara-effects-for-unreal-engine) is Unreal's VFX system for the dust/sparks, [**Sound Cues**](https://dev.epicgames.com/documentation/en-us/unreal-engine/sound-cues-in-unreal-engine) add pitch variation over raw waves, and [UMG animations](https://dev.epicgames.com/documentation/en-us/unreal-engine/animating-umg-widgets-in-unreal-engine) punch up the UI. The final [**Shipping**](https://dev.epicgames.com/documentation/en-us/unreal-engine/packaging-your-project) build is the optimized, stripped, distributable v1.0.

🔑 **Key nodes / functions:** `Widget Animation (Designer) · Sound Cue asset · UNiagaraFunctionLibrary::SpawnSystemAtLocation · Build.cs += "Niagara" · Package Project (Shipping) · Project Settings→Description icon/splash`

### 🧩 C++ classes & methods
- Add `UPROPERTY(EditAnywhere) TObjectPtr<class UNiagaraSystem> BreakVFX;` to `AOreVein` (and dust/spark VFX for crits, mineral shimmer, coin poof); spawn via `UNiagaraFunctionLibrary::SpawnSystemAtLocation`. Add **`"Niagara"`** to `Build.cs`.
- Optionally convert the raw `PlaySound2D` calls to **Sound Cues** (still `USoundBase` UPROPERTYs — assign a Sound Cue instead of a Sound Wave).

### 🔵 Blueprints
- Assign Niagara systems + Sound Cues on the relevant `BP_`/data assets; author UMG animations in `WBP_HUD`/`WBP_TechNode` (DIG punch, purchase pop, unlock flourish).

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_OreVein` | `BreakVFX` | a Niagara dust burst |
| `BP_Coin` | `PickupSound` | `SC_CoinPickup` (Sound Cue) |

### Steps
1. UMG animations; Niagara bursts; ambient/music.
2. Set icon/splash/name; **Shipping** config; **Package**; test end-to-end; share (friend or itch.io).

🏁 **Milestone:** a polished, playable v1.0.
🧪 **Boss check:** the Shipping build plays the whole game with sound + effects.
🎁 **Reward:** **Deeplord, Shaper of the Deep** · +400 XP · 🏅 QUEST LINE COMPLETE.

**🎓 What to learn next:** Gameplay Ability System (GAS), replication & multiplayer, and AI / Behavior Trees.

---

*Engine: Unreal Engine 5.8 · IDE: Rider · Art: Synty POLYGON Dungeon Realms · Docs: dev.epicgames.com. The C++-first chain is fully specified and dependency-audited (self-contained).*
