# 🟢 DEEP DELVE — C++-First Quest Log

> The **C++-first** path: every system is a C++ class from Quest 1, with a thin **Blueprint subclass** that only assigns data (meshes, sounds, Data Assets, input assets) and sets `UPROPERTY` defaults. UMG uses C++ `UUserWidget` base classes with `meta=(BindWidget)` controls and `WBP_` layouts.
>
> **Engine:** Unreal Engine 5.8 · **IDE:** JetBrains Rider · **Art:** Synty POLYGON Dungeon Realms · **API macro used below:** `DEEPDELVE_API` (replace with your module's API macro).
>
> ← Blueprints-first path: https://fdvoracek.github.io/deep-delve/blueprints/

## The working pattern (read once)

For every class we build:
1. **Write the C++ class** — `UPROPERTY` members expose data; specifiers decide who edits them:
   - `EditDefaultsOnly` — set once per class in the **Blueprint's Class Defaults** (meshes, sounds, data-asset refs, tuning).
   - `EditAnywhere` — editable on defaults *and* per placed instance.
   - `VisibleAnywhere`/`BlueprintReadOnly` — runtime state you don't hand-edit.
   - `BlueprintCallable`/`BlueprintNativeEvent` — callable/overridable from Blueprints.
2. **Create a Blueprint subclass** (`BP_…` / `WBP_…`) — this is where you *assign the values* to those `UPROPERTY`s and drop the actor into the level.
3. **Data lives in `UPrimaryDataAsset`s** (`DA_…` instances) referenced by the classes.

Each quest below lists: the **C++ classes/methods** created or updated, the **Blueprint(s)** to create, and a **UPROPERTY → value** table for what you assign in Blueprints.

## The C++-first arc

1. **Foundations & First Class** — C++ project, Rider, migrate Synty, `ADeepDelveGameMode`
2. **The Player & Enhanced Input** — `ADeepDelvePlayerController` (+ `Damage`, `IA_Dig`)
3. **The Rock & the Interface** — `IMineable`, `AOreVein`, click→`TakeDamage`, first sounds
4. **Coins & the Economy Subsystem** — `UMineEconomySubsystem`, `ACoin`, `UHUDWidget`, damage numbers
5. **Data-Driven Content** — `URockData`, `UPickaxeData` (+ `DA_` instances)
6. Tech Tree — Model (`UTechNode`, polymorphic `UTechEffect`, Buy/Apply) *(Batch 2)*
7. Tech Tree — UI (`UTechTreeWidget`/`UTechNodeWidget`) *(Batch 2)*
8. Hire the Crew (`ADwarf`, `UMinerData`) *(Batch 2)*
9. Menus & Level Flow (`UMenuWidget`) *(Batch 2)*
10. Save & Load (`UDeepDelveSaveGame`) *(Batch 2)*
11. Ship a Demo *(Batch 3)*
12. Pickaxe upgrades (auto-swing, crits) *(Batch 3)*
13. The Growing Crew *(Batch 3)*
14. Rich Veins (`AMineral`) *(Batch 3)*
15. The Coin Collector (`ACollectorDwarf`) *(Batch 3)*
16. Strike It Rich (juice, Niagara, Shipping v1.0) *(Batch 3)*

> **Status:** Quests 1–5 fully specified below. Quests 6–16 arrive in Batches 2–3.

---

# QUEST 1 — Foundations & First Class · ~60 min

**Goal.** Stand up a **C++** project, wire up Rider, migrate the Synty assets, compose the mine scene, and create your first C++ class — the GameMode — with its Blueprint.

🎮 **After:** the project compiles from Rider, your mine scene renders through a fixed camera, and your own `ADeepDelveGameMode` (via `BP_DeepDelveGameMode`) is the active game mode.

### 🧩 C++ classes & methods
**Create `ADeepDelveGameMode : AGameModeBase`.**
```cpp
// DeepDelveGameMode.h
#pragma once
#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "DeepDelveGameMode.generated.h"

UCLASS()
class DEEPDELVE_API ADeepDelveGameMode : public AGameModeBase
{
    GENERATED_BODY()
public:
    ADeepDelveGameMode();
};
```
```cpp
// DeepDelveGameMode.cpp
#include "DeepDelveGameMode.h"

ADeepDelveGameMode::ADeepDelveGameMode()
{
    // Click game — no player pawn. We drive everything from the PlayerController (Quest 2).
    DefaultPawnClass = nullptr;
    // PlayerControllerClass is assigned in the Blueprint in Quest 2.
}
```

### 🔵 Blueprint to create
- **`BP_DeepDelveGameMode`** — Blueprint Class, parent = `ADeepDelveGameMode`. No values to set yet. Then **Project Settings → Maps & Modes → Default GameMode = `BP_DeepDelveGameMode`** (and set it in the `Mine` map's **World Settings** too).

### 🎛️ UPROPERTY → Blueprint values
*(none this quest)*

### Steps
1. **Install** UE 5.8, **JetBrains Rider**, and the compiler toolchain (Windows: *Visual Studio Build Tools* with the C++ workload; macOS: Xcode command-line tools). In Rider/UE this is the C++ compiler UBT calls.
2. **New project → Games → Blank → C++** (not Blueprint), Desktop, Starter Content off. Name it `DeepDelve`. UE generates a C++ module and opens your IDE.
3. In UE: **Editor Preferences → General → Source Code → Source Code Editor = Rider**. Generate project files if prompted (right-click the `.uproject` → *Generate…*, or from Rider).
4. **Build** the empty project from Rider (Build Solution) and confirm the editor opens — this proves your toolchain works.
5. **Migrate Synty:** unzip the Synty pack (a UE project), open it in 5.8 (convert a copy if older), right-click its Content folder → **Asset Actions → Migrate** → choose your `DeepDelve/Content`.
6. Compose the `Mine` level (Synty tiles + a dwarf + light + a **Camera Actor** with *Auto Activate for Player 0*), same as the shared setup.
7. **Tools → New C++ Class → GameModeBase** → name `DeepDelveGameMode`. Paste the code above, build from Rider.
8. Create **`BP_DeepDelveGameMode`** and set it as the Default GameMode.

🏁 **Milestone:** clean compile from Rider; the `Mine` scene plays through the camera with your GameMode active.
🧪 **Boss check:** add a temporary `UE_LOG(LogTemp, Warning, TEXT("GameMode up"))` in the constructor, rebuild, and see it in the **Output Log** on play.
🎁 **Reward:** Title **Prospector** · +100 XP.

---

# QUEST 2 — The Player & Enhanced Input · ~55 min

**Goal.** Create the Player Controller in C++, give it the player's `Damage` stat, and wire **Enhanced Input** entirely in code so a left-click fires an `OnDig` handler.

🎮 **After:** clicking logs a "dig" from your C++ controller; the player's `Damage` lives on the controller and is set in its Blueprint.

### 🧩 C++ classes & methods
**Create `ADeepDelvePlayerController : APlayerController`.**
```cpp
// DeepDelvePlayerController.h
#pragma once
#include "CoreMinimal.h"
#include "GameFramework/PlayerController.h"
#include "DeepDelvePlayerController.generated.h"

class UInputMappingContext;
class UInputAction;

UCLASS()
class DEEPDELVE_API ADeepDelvePlayerController : public APlayerController
{
    GENERATED_BODY()
public:
    // Player power. Tuned in the Blueprint defaults.
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Stats")
    float Damage = 1.f;

protected:
    UPROPERTY(EditDefaultsOnly, Category="Input")
    TObjectPtr<UInputMappingContext> MiningContext;

    UPROPERTY(EditDefaultsOnly, Category="Input")
    TObjectPtr<UInputAction> DigAction;

    virtual void BeginPlay() override;
    virtual void SetupInputComponent() override;

    void OnDig();   // bound to DigAction
};
```
```cpp
// DeepDelvePlayerController.cpp
#include "DeepDelvePlayerController.h"
#include "EnhancedInputComponent.h"
#include "EnhancedInputSubsystems.h"

void ADeepDelvePlayerController::BeginPlay()
{
    Super::BeginPlay();
    bShowMouseCursor = true;
    if (auto* Subsystem = ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem>(GetLocalPlayer()))
        if (MiningContext) Subsystem->AddMappingContext(MiningContext, 0);

    SetInputMode(FInputModeGameAndUI());   // world clicks AND (later) UI both work
}

void ADeepDelvePlayerController::SetupInputComponent()
{
    Super::SetupInputComponent();
    if (auto* EIC = Cast<UEnhancedInputComponent>(InputComponent))
        if (DigAction) EIC->BindAction(DigAction, ETriggerEvent::Triggered, this, &ADeepDelvePlayerController::OnDig);
}

void ADeepDelvePlayerController::OnDig()
{
    // Quest 3 will trace under the cursor and deal damage. For now, prove input works:
    UE_LOG(LogTemp, Warning, TEXT("Dig! Damage=%.0f"), Damage);
}
```
- Add `"EnhancedInput"` to your module's `PublicDependencyModuleNames` in `DeepDelve.Build.cs`.

### 🔵 Blueprint(s) to create
- **`BP_MineController`** — parent `ADeepDelvePlayerController`.
- In the editor create the input assets: **`IMC_Mining`** (Input Mapping Context) and **`IA_Dig`** (Input Action, value type Digital/bool). In `IMC_Mining`, map `IA_Dig` to **Left Mouse Button**.
- Set **`BP_DeepDelveGameMode` → Player Controller Class = `BP_MineController`**.

### 🎛️ UPROPERTY → Blueprint values (on `BP_MineController`)
| UPROPERTY | Specifier | Assign in Blueprint |
|---|---|---|
| `MiningContext` | EditDefaultsOnly | `IMC_Mining` |
| `DigAction` | EditDefaultsOnly | `IA_Dig` |
| `Damage` | EditAnywhere | `1.0` (starting pick power) |

### Steps
1. New C++ class → **PlayerController** → `DeepDelvePlayerController`; paste the code; add `EnhancedInput` to `Build.cs`; build.
2. Create `IMC_Mining` + `IA_Dig`; map Left Mouse Button.
3. Create `BP_MineController`, assign the table above.
4. Point `BP_DeepDelveGameMode`'s Player Controller Class at `BP_MineController`.
5. Play, click, watch the log.

🏁 **Milestone:** left-click prints `Dig! Damage=1` from C++.
🧪 **Boss check:** set `Damage = 5` on `BP_MineController` and confirm the log reads 5 — proving the Blueprint feeds the C++ value.
🎁 **Reward:** Title **Pit Digger** · +150 XP.

---

# QUEST 3 — The Rock & the Interface · ~55 min

**Goal.** A reusable **`IMineable`** C++ interface, the **`AOreVein`** actor that implements it, and the controller's cursor trace that calls `TakeDamage`. First hit/break sounds.

🎮 **After:** clicking the rock damages it via the interface; at 0 HP it breaks (with sound) and a fresh rock spawns.

### 🧩 C++ classes & methods
**Create the interface `UMineable` / `IMineable`.**
```cpp
// Mineable.h
#pragma once
#include "CoreMinimal.h"
#include "UObject/Interface.h"
#include "Mineable.generated.h"

UINTERFACE(BlueprintType)
class UMineable : public UInterface { GENERATED_BODY() };

class IMineable
{
    GENERATED_BODY()
public:
    // BlueprintNativeEvent: C++ provides _Implementation; Blueprints may also respond.
    UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category="Mining")
    void TakeDamage(float Amount);
};
```
**Create `AOreVein : AActor`, implementing `IMineable`.**
```cpp
// OreVein.h
UCLASS()
class DEEPDELVE_API AOreVein : public AActor, public IMineable
{
    GENERATED_BODY()
public:
    AOreVein();

    UPROPERTY(VisibleAnywhere) TObjectPtr<UStaticMeshComponent> Mesh;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Rock") float MaxHP = 10.f;
    UPROPERTY(VisibleInstanceOnly, BlueprintReadOnly, Category="Rock") float HP = 10.f;

    UPROPERTY(EditAnywhere, Category="Rock|FX") TObjectPtr<USoundBase> HitSound;
    UPROPERTY(EditAnywhere, Category="Rock|FX") TObjectPtr<USoundBase> BreakSound;
    UPROPERTY(EditAnywhere, Category="Rock") TSubclassOf<AOreVein> RespawnClass;

    virtual void BeginPlay() override;
    virtual void TakeDamage_Implementation(float Amount) override;  // IMineable
};
```
```cpp
// OreVein.cpp (key bodies)
AOreVein::AOreVein(){ Mesh = CreateDefaultSubobject<UStaticMeshComponent>("Mesh"); RootComponent = Mesh; }
void AOreVein::BeginPlay(){ Super::BeginPlay(); HP = MaxHP; }
void AOreVein::TakeDamage_Implementation(float Amount)
{
    HP -= Amount;
    if (HitSound) UGameplayStatics::PlaySound2D(this, HitSound);
    if (HP <= 0.f)
    {
        if (BreakSound) UGameplayStatics::PlaySound2D(this, BreakSound);
        if (RespawnClass) GetWorld()->SpawnActor<AOreVein>(RespawnClass, GetActorTransform());
        Destroy();
    }
}
```
**Update `ADeepDelvePlayerController::OnDig()`** — trace under cursor, call the interface:
```cpp
#include "Mineable.h"
void ADeepDelvePlayerController::OnDig()
{
    FHitResult Hit;
    if (GetHitResultUnderCursor(ECC_Visibility, false, Hit))
        if (AActor* A = Hit.GetActor())
            if (A->Implements<UMineable>())
                IMineable::Execute_TakeDamage(A, Damage);
}
```

### 🔵 Blueprint(s) to create
- **`BP_OreVein`** — parent `AOreVein`. Place one in the `Mine` level where the camera looks.
- Import two short sounds as `USoundWave`s (e.g. `S_RockHit`, `S_RockBreak`).

### 🎛️ UPROPERTY → Blueprint values (on `BP_OreVein`)
| UPROPERTY | Specifier | Assign in Blueprint |
|---|---|---|
| `Mesh` (component) | VisibleAnywhere | Static Mesh = a Synty rock mesh |
| `MaxHP` | EditAnywhere | `10` |
| `HitSound` | EditAnywhere | `S_RockHit` |
| `BreakSound` | EditAnywhere | `S_RockBreak` |
| `RespawnClass` | EditAnywhere | `BP_OreVein` (itself) |

### Steps
1. New C++ class → **Unreal Interface** → `Mineable`; paste.
2. New C++ class → **Actor** → `OreVein`, add `public IMineable`; paste; build.
3. Update `OnDig()` to trace + call `Execute_TakeDamage`.
4. Create `BP_OreVein`, assign the table, place it in the level.

🏁 **Milestone:** ten clicks break the rock (with sound) and a new one appears.
🧪 **Boss check:** in `BP_OreVein` set `MaxHP = 20` and confirm it now takes 20 clicks — data flowing C++ ↔ Blueprint.
🎁 **Reward:** Title stays **Pit Digger** · +200 XP.
💡 *Gotcha:* the rock needs collision that the cursor trace can hit; the default StaticMesh collision on the Synty mesh is usually fine (Block Visibility).

---

# QUEST 4 — Coins & the Economy Subsystem · ~60 min

**Goal.** A **`UGameInstanceSubsystem`** for the economy, a collectible **`ACoin`**, a C++ **HUD widget** with `BindWidget`, and floating damage numbers.

🎮 **After:** broken rocks burst coins you sweep up (with a "ching"); a HUD shows Coins + Depth; a "1" floats off each hit.

### 🧩 C++ classes & methods
**Create `UMineEconomySubsystem : UGameInstanceSubsystem`** (no Blueprint needed — auto-instantiated).
```cpp
// MineEconomySubsystem.h
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCoinsChanged);

UCLASS()
class DEEPDELVE_API UMineEconomySubsystem : public UGameInstanceSubsystem
{
    GENERATED_BODY()
public:
    UPROPERTY(BlueprintReadOnly, Category="Economy") double Coins = 0;
    UPROPERTY(BlueprintReadOnly, Category="Economy") double Depth = 0;
    UPROPERTY(BlueprintAssignable) FOnCoinsChanged OnCoinsChanged;

    UFUNCTION(BlueprintCallable) void AddCoins(double Amount);
    UFUNCTION(BlueprintCallable) void AddDepth(double Amount);
};
// .cpp: AddCoins → Coins += Amount; OnCoinsChanged.Broadcast();
```
**Create `UMineStatics : UBlueprintFunctionLibrary`** for number formatting:
```cpp
UFUNCTION(BlueprintPure, Category="UI") static FText FormatNumber(double Value); // 1500 -> "1.5K"
```
**Create `ACoin : AActor`.**
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<UStaticMeshComponent> Mesh; // Simulate Physics = true (set in BP)
UPROPERTY(EditAnywhere, Category="Coin") int32 Value = 1;
UPROPERTY(EditAnywhere, Category="Coin") float LaunchImpulse = 300.f;
UPROPERTY(EditAnywhere, Category="Coin") float Lifespan = 30.f;
UPROPERTY(EditAnywhere, Category="Coin|FX") TObjectPtr<USoundBase> PickupSound;

virtual void BeginPlay() override;                 // random Add Impulse; SetLifeSpan(Lifespan)
virtual void NotifyActorBeginCursorOver() override; // -> Collect()
void Collect();                                     // subsystem->AddCoins(Value); PlaySound2D; Destroy
```
**Create `UHUDWidget : UUserWidget`.**
```cpp
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UTextBlock> CoinsText;
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UTextBlock> DepthText;
virtual void NativeConstruct() override;  // bind subsystem->OnCoinsChanged to Refresh(); call Refresh()
UFUNCTION() void Refresh();               // CoinsText->SetText(UMineStatics::FormatNumber(Coins)) etc.
```
**Update `AOreVein`** — spawn coins on break:
```cpp
UPROPERTY(EditAnywhere, Category="Rock") int32 CoinReward = 10;
UPROPERTY(EditAnywhere, Category="Rock") TSubclassOf<ACoin> CoinClass;
// in TakeDamage_Implementation break branch: loop CoinReward -> SpawnActor<ACoin>(CoinClass, ...); Economy->AddDepth(1);
```
**Update `ADeepDelvePlayerController::BeginPlay`** — `bEnableMouseOverEvents = true;` and create + add `UHUDWidget` to the viewport (`CreateWidget`, expose the widget class as a `UPROPERTY(EditDefaultsOnly) TSubclassOf<UHUDWidget> HUDClass;`).

### 🔵 Blueprints to create
- **`BP_Coin`** (parent `ACoin`), **`WBP_HUD`** (parent `UHUDWidget`), **`WBP_DamageNumber`** (a small `UUserWidget` with a float-up+fade animation).

### 🎛️ UPROPERTY → Blueprint values
| Class | UPROPERTY | Assign in Blueprint |
|---|---|---|
| `BP_Coin` | `Mesh` | Synty gold-coin mesh; **Simulate Physics = true** |
| `BP_Coin` | `Value` | `1` |
| `BP_Coin` | `PickupSound` | `S_CoinPickup` |
| `BP_Coin` | `LaunchImpulse` / `Lifespan` | `300` / `30` |
| `WBP_HUD` | `CoinsText`, `DepthText` | name two Text Blocks exactly `CoinsText` / `DepthText` (satisfies `BindWidget`) |
| `BP_MineController` | `HUDClass` | `WBP_HUD` |
| `BP_OreVein` | `CoinReward` | `10` |
| `BP_OreVein` | `CoinClass` | `BP_Coin` |

### Steps
1. Create the subsystem, `UMineStatics`, `ACoin`, `UHUDWidget`; build.
2. Update `AOreVein` (coin fields + spawn loop) and the controller (mouse-over + spawn HUD).
3. Create `BP_Coin`, `WBP_HUD` (name the Text Blocks to match `BindWidget`), `WBP_DamageNumber`.
4. Assign the table; play.

🏁 **Milestone:** break → coins burst → hover to collect → HUD climbs, Depth ticks, damage numbers float.
🧪 **Boss check:** confirm a coin credits exactly once and the HUD total matches.
🎁 **Reward:** Title **Miner** · +200 XP.
💡 *Gotcha:* `BindWidget` requires the `WBP_HUD` widget names to **exactly match** the C++ member names, or the widget won't compile.

---

# QUEST 5 — Data-Driven Content · ~50 min

**Goal.** Move rock and pickaxe stats into **`UPrimaryDataAsset`s** so new content is data, not code.

🎮 **After:** identical to play, but a new rock/pickaxe is a new `DA_` asset — no recompile.

### 🧩 C++ classes & methods
**Create `URockData` and `UPickaxeData : UPrimaryDataAsset`.**
```cpp
UCLASS() class DEEPDELVE_API URockData : public UPrimaryDataAsset {
    GENERATED_BODY()
public:
    UPROPERTY(EditAnywhere, Category="Rock") TObjectPtr<UStaticMesh> Mesh;
    UPROPERTY(EditAnywhere, Category="Rock") int32 MaxHP = 10;
    UPROPERTY(EditAnywhere, Category="Rock") int32 CoinReward = 10;
};
UCLASS() class DEEPDELVE_API UPickaxeData : public UPrimaryDataAsset {
    GENERATED_BODY()
public:
    UPROPERTY(EditAnywhere, Category="Pickaxe") TObjectPtr<UStaticMesh> Mesh;
    UPROPERTY(EditAnywhere, Category="Pickaxe") float Damage = 1.f;
    UPROPERTY(EditAnywhere, Category="Pickaxe") float Speed = 1.f;
};
```
**Update `AOreVein`** — drive from data:
```cpp
UPROPERTY(EditAnywhere, Category="Rock") TObjectPtr<URockData> RockData;
// BeginPlay: if (RockData){ Mesh->SetStaticMesh(RockData->Mesh); MaxHP = RockData->MaxHP; CoinReward = RockData->CoinReward; } HP = MaxHP;
```
**Update `ADeepDelvePlayerController`** — equip pickaxe data:
```cpp
UPROPERTY(EditAnywhere, Category="Stats") TObjectPtr<UPickaxeData> EquippedPickaxe;
// effective damage: float GetDamage() const { return EquippedPickaxe ? EquippedPickaxe->Damage : Damage; }
// OnDig now sends GetDamage()
```

### 🔵 Data Assets & Blueprint values
- Create **`DA_CopperVein`** (`URockData`): Mesh = copper rock, MaxHP `10`, CoinReward `10`. **`DA_IronVein`**: MaxHP `25`, CoinReward `30`.
- Create **`DA_StonePick`** (`UPickaxeData`): Damage `1`. **`DA_SturdyPick`**: Damage `3`.

| Class | UPROPERTY | Assign in Blueprint |
|---|---|---|
| `BP_OreVein` | `RockData` | `DA_CopperVein` (replaces the per-value mesh/HP you set in Q3) |
| `BP_MineController` | `EquippedPickaxe` | `DA_StonePick` |

### Steps
1. Create `URockData`, `UPickaxeData`; build. Create the `DA_` instances (right-click → Miscellaneous → Data Asset → pick the class).
2. Update `AOreVein::BeginPlay` and the controller's damage source.
3. In `BP_OreVein` assign `RockData = DA_CopperVein`; in `BP_MineController` assign `EquippedPickaxe = DA_StonePick`.
4. Prove it: point `BP_OreVein`'s `RockData` at `DA_IronVein` and watch it get tougher/richer with zero recompile.

🏁 **Milestone:** a new rock type + pickaxe added purely as `DA_` assets.
🧪 **Boss check:** create a third `URockData` in under two minutes and slot it in.
🎁 **Reward:** Title stays **Miner** · +200 XP.

---

*Batches 2 (Q6–10) and 3 (Q11–16) follow — tech tree, crew, menus, save/load, demo, upgrades, veins, collector, and the v1.0 finale, all in C++ with matching Blueprints.*

*Engine: Unreal Engine 5.8 · IDE: Rider · Art: Synty POLYGON Dungeon Realms · Docs: dev.epicgames.com*
