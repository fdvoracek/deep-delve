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
6. Tech Tree — Model (`UTechNode`, polymorphic `UTechEffect`, Buy/Apply)
7. Tech Tree — UI (`UTechTreeWidget`/`UTechNodeWidget`)
8. Hire the Crew (`ADwarf`, `UMinerData`)
9. Menus & Level Flow (`UMenuWidget`)
10. Save & Load (`UDeepDelveSaveGame`)
11. Ship a Demo
12. Pickaxe upgrades (auto-swing, crits)
13. The Growing Crew
14. Rich Veins (`AMineral`)
15. The Coin Collector (`ACollectorDwarf`)
16. Strike It Rich (juice, Niagara, Shipping v1.0)

> **Status:** all 16 quests specified below (first migration pass — to be refined next).

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

# QUEST 6 — Tech Tree: the Model · ~65 min

**Goal.** A data-driven tech tree with a **polymorphic `UTechEffect`** hierarchy that drives every upgrade. All gameplay modifiers live on the economy subsystem so effects only touch one object.

🎮 **After:** you can buy the first three nodes (via a temporary button, UI comes next quest): Sharpen Pickaxe → Iron Pickaxe → Descend.

### 🧩 C++ classes & methods
**Create `UTechEffect : UObject`** (base — instanced, inline-editable, so each node owns its effect object):
```cpp
UCLASS(Abstract, EditInlineNew, DefaultToInstanced, BlueprintType)
class DEEPDELVE_API UTechEffect : public UObject
{
    GENERATED_BODY()
public:
    UFUNCTION() virtual void Apply(class UMineEconomySubsystem* Econ, int32 NewLevel) {}
};
```
**Create subclasses** (each with its own `UPROPERTY` params + `Apply` override):
```cpp
UCLASS() class UTechEffect_UpgradePickaxeDamage : public UTechEffect { GENERATED_BODY()
  UPROPERTY(EditAnywhere) float AmountPerLevel = 1.f;
  virtual void Apply(UMineEconomySubsystem* Econ, int32 NewLevel) override; }; // Econ->PickaxeDamageBonus += AmountPerLevel;
UCLASS() class UTechEffect_EquipPickaxe : public UTechEffect { GENERATED_BODY()
  UPROPERTY(EditAnywhere) TObjectPtr<class UPickaxeData> Pickaxe;
  virtual void Apply(...) override; }; // Econ->EquippedPickaxe = Pickaxe;
UCLASS() class UTechEffect_Descend : public UTechEffect { GENERATED_BODY()
  UPROPERTY(EditAnywhere) TObjectPtr<class URockData> NextRock;
  virtual void Apply(...) override; }; // Econ->SetCurrentRock(NextRock); (also nudges the camera)
```
**Create `UTechNode : UPrimaryDataAsset`:**
```cpp
UPROPERTY(EditAnywhere) FName NodeId;
UPROPERTY(EditAnywhere) FText DisplayName;
UPROPERTY(EditAnywhere) FText Description;
UPROPERTY(EditAnywhere) TObjectPtr<UTexture2D> Icon;
UPROPERTY(EditAnywhere) int32 Cost = 10;
UPROPERTY(EditAnywhere) float CostGrowth = 1.15f;
UPROPERTY(EditAnywhere) bool bRepeatable = false;
UPROPERTY(EditAnywhere) int32 MaxLevel = 1;
UPROPERTY(EditAnywhere) TArray<TObjectPtr<UTechNode>> Prerequisites;
UPROPERTY(EditAnywhere) FVector2D GridPosition;
UPROPERTY(EditAnywhere, Instanced) TObjectPtr<UTechEffect> Effect;   // inline effect object
```
**Update `UMineEconomySubsystem`** — tech state + gameplay fields effects write to:
```cpp
UPROPERTY() TMap<FName,int32> NodeLevels;
UPROPERTY(BlueprintReadOnly) float PickaxeDamageBonus = 0.f;
UPROPERTY(BlueprintReadOnly) TObjectPtr<UPickaxeData> EquippedPickaxe;
UPROPERTY(BlueprintReadOnly) TObjectPtr<URockData> CurrentRock;
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnTechChanged);
UPROPERTY(BlueprintAssignable) FOnTechChanged OnTechChanged;

UFUNCTION(BlueprintCallable) int32 GetNodeLevel(UTechNode* Node) const;
UFUNCTION(BlueprintCallable) int32 GetCost(UTechNode* Node) const;      // Cost * CostGrowth^level
UFUNCTION(BlueprintCallable) bool  CanBuy(UTechNode* Node) const;       // prereqs met, not maxed, Coins>=cost
UFUNCTION(BlueprintCallable) bool  TryBuy(UTechNode* Node);             // spend, level++, Effect->Apply, broadcast
```
The player's effective damage becomes `EquippedPickaxe->Damage + Econ->PickaxeDamageBonus`.

### 🔵 Data Assets to create (no BP subclasses — effects are inline on the node)
- `DA_Node_Sharpen` (`UTechNode`), `DA_Node_IronPick`, `DA_Node_Descend`.

### 🎛️ UPROPERTY → values
| Data Asset | Field | Value |
|---|---|---|
| `DA_Node_Sharpen` | `bRepeatable` / `Cost` / `CostGrowth` | `true` / `10` / `1.15` |
| `DA_Node_Sharpen` | `Effect` | `UTechEffect_UpgradePickaxeDamage` (AmountPerLevel `1`) |
| `DA_Node_IronPick` | `Prerequisites` / `Cost` | `[DA_Node_Sharpen]` / `100` |
| `DA_Node_IronPick` | `Effect` | `UTechEffect_EquipPickaxe` (Pickaxe = `DA_SturdyPick`) |
| `DA_Node_Descend` | `Prerequisites` / `Cost` | `[DA_Node_IronPick]` / `250` |
| `DA_Node_Descend` | `Effect` | `UTechEffect_Descend` (NextRock = `DA_IronVein`) |

🏁 **Milestone:** buying the three nodes (temporary button → `TryBuy`) sharpens, equips, and descends. 🧪 **Boss:** add a 4th node as a `DA_` + one new effect subclass. 🎁 **Reward:** Pit Foreman · +300 XP.
💡 *Gotcha:* mark `UTechEffect` `EditInlineNew` + `DefaultToInstanced` and the node's `Effect` property `Instanced`, or you can't create effect objects inline on the Data Asset.

---

# QUEST 7 — Tech Tree: the UI · ~60 min

**Goal.** A real tech-tree screen built from C++ `UUserWidget` bases + `WBP_` layouts.

🎮 **After:** a screen of connected nodes that light up when affordable and check off when bought.

### 🧩 C++ classes & methods
**`UTechNodeWidget : UUserWidget`**
```cpp
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UButton> BuyButton;
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UImage> IconImage;
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UTextBlock> NameText, CostText;
UPROPERTY() TObjectPtr<UTechNode> Node;
void Setup(UTechNode* InNode);   // fill icon/name; bind BuyButton->OnClicked
UFUNCTION() void OnBuyClicked(); // Econ->TryBuy(Node)
UFUNCTION() void Refresh();      // colour by CanBuy/owned/maxed; SetIsEnabled
```
**`UTechTreeWidget : UUserWidget`**
```cpp
UPROPERTY(meta=(BindWidget)) TObjectPtr<class UCanvasPanel> Canvas;
UPROPERTY(EditDefaultsOnly) TSubclassOf<UTechNodeWidget> NodeWidgetClass;
UPROPERTY(EditDefaultsOnly) TArray<TObjectPtr<UTechNode>> AllNodes;
virtual void NativeConstruct() override;  // spawn a node widget per AllNodes at GridPosition; bind Econ->OnTechChanged -> RefreshAll
virtual int32 NativePaint(...) const override; // draw connector lines from each node's Prerequisites
```

### 🔵 Blueprints
- **`WBP_TechNode`** (parent `UTechNodeWidget`) — layout with widgets named `BuyButton`, `IconImage`, `NameText`, `CostText`.
- **`WBP_TechTree`** (parent `UTechTreeWidget`) — a Canvas Panel named `Canvas`.

### 🎛️ UPROPERTY → values
| Widget | Field | Value |
|---|---|---|
| `WBP_TechTree` | `NodeWidgetClass` | `WBP_TechNode` |
| `WBP_TechTree` | `AllNodes` | `[DA_Node_Sharpen, DA_Node_IronPick, DA_Node_Descend]` |

🏁 **Milestone:** the visual tree; click affordable nodes to buy. 🧪 **Boss:** add the 4th node's `GridPosition`+prereq and see it wired with no layout code. 🎁 **Reward:** Pit Foreman · +300 XP.

---

# QUEST 8 — Hire the Crew · ~60 min

**Goal.** An animated `ADwarf` that mines the rock on a timer via the `IMineable` interface, hired through a tech node.

🎮 **After:** buy "Hire Dwarf" → a dwarf appears and mines the rock on his own.

### 🧩 C++ classes & methods
**`UMinerData : UPrimaryDataAsset`** — `USkeletalMesh* Mesh; TObjectPtr<UAnimationAsset> MineAnim; float Damage; float Speed;`
**`ADwarf : AActor`**
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<USkeletalMeshComponent> Mesh;
UPROPERTY(EditAnywhere) TObjectPtr<UMinerData> MinerData;
UPROPERTY(EditAnywhere) TObjectPtr<USoundBase> HitSound;
FTimerHandle SwingTimer;
virtual void BeginPlay() override;  // set skeletal mesh + play MineAnim; start looping timer at MinerData->Speed
void Swing();                       // Econ->CurrentRockActor -> IMineable::Execute_TakeDamage(rock, MinerData->Damage * Econ->CrewDamageMult); PlaySound2D; spawn damage number
```
**`UTechEffect_HireMiner : UTechEffect`** — `TObjectPtr<UMinerData> Miner; TSubclassOf<ADwarf> DwarfClass;` → `Apply` calls `Econ->SpawnDwarf(DwarfClass, Miner)`.
**Update `UMineEconomySubsystem`** — `TObjectPtr<AActor> CurrentRockActor; float CrewDamageMult = 1.f;` + `void SpawnDwarf(TSubclassOf<ADwarf>, UMinerData*)` (spawns at a ring slot). `AOreVein::BeginPlay` registers itself as `CurrentRockActor`.

### 🔵 Blueprints / Data
- **`BP_Dwarf`** (parent `ADwarf`), **`DA_Dwarf1`** (`UMinerData`), **`DA_Node_HireDwarf`** (`UTechNode`, repeatable).

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Dwarf1` | `Mesh` / `MineAnim` / `Damage` / `Speed` | Synty dwarf skeletal mesh / mining anim / `1` / `1.0` |
| `BP_Dwarf` | `MinerData` / `HitSound` | `DA_Dwarf1` / `S_RockHit` |
| `DA_Node_HireDwarf` | `Effect` | `UTechEffect_HireMiner` (Miner=`DA_Dwarf1`, DwarfClass=`BP_Dwarf`) |

🏁 **Milestone:** a hired dwarf mines hands-free. 🧪 **Boss:** hire two; both damage the rock independently. 🎁 **Reward:** Master Smith · +300 XP.

---

# QUEST 9 — Menus & Level Flow · ~55 min

**Goal.** A Main Menu level and an Esc pause menu, driven by a C++ `UMenuWidget`.

🎮 **After:** the game boots to a title screen (New Game / Quit); Esc pauses in the mine.

### 🧩 C++ classes & methods
**`UMenuWidget : UUserWidget`**
```cpp
UFUNCTION(BlueprintCallable) void NewGame();       // Econ->ResetToDefaults(); UGameplayStatics::OpenLevel("Mine")
UFUNCTION(BlueprintCallable) void Continue();      // (Q10) Econ->LoadGame(); OpenLevel("Mine")
UFUNCTION(BlueprintCallable) void QuitToMenu();    // OpenLevel("MainMenu")
UFUNCTION(BlueprintCallable) void QuitGame();      // UKismetSystemLibrary::QuitGame
```
**Update `ADeepDelvePlayerController`** — add `IA_Pause`; on it, create/remove a pause widget, `SetInputMode`, `SetPause(true/false)`; expose `TSubclassOf<UMenuWidget> PauseMenuClass` (`EditDefaultsOnly`). A tiny `AMainMenuController`/level-BP creates `WBP_MainMenu` in the MainMenu map.
**Update `UMineEconomySubsystem`** — `void ResetToDefaults()` (clears Coins/Depth/NodeLevels/dwarves).

### 🔵 Blueprints / Levels
- **`WBP_MainMenu`**, **`WBP_PauseMenu`** (parent `UMenuWidget`), a **MainMenu** level. Set **Game Default Map = MainMenu**.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_MineController` | `IA_Pause` | `IA_Pause` (Esc) |
| `BP_MineController` | `PauseMenuClass` | `WBP_PauseMenu` |

🏁 **Milestone:** title → New Game → mine; Esc pauses/returns. 🧪 **Boss:** New Game twice; the second run is fresh (`ResetToDefaults`). 🎁 **Reward:** Master Smith · +250 XP.

---

# QUEST 10 — Save & Load · ~60 min

**Goal.** A `USaveGame` subclass + save/load in the subsystem that **reconstructs the world** (dwarves, tier, pickaxe).

🎮 **After:** quit, relaunch, Continue → everything is back.

### 🧩 C++ classes & methods
**`UDeepDelveSaveGame : USaveGame`**
```cpp
UPROPERTY() int32 SaveVersion = 1;
UPROPERTY() double Coins = 0, Depth = 0;
UPROPERTY() TMap<FName,int32> NodeLevels;
UPROPERTY() int32 DwarfCount = 0;
UPROPERTY() FString CurrentRockId, EquippedPickaxeId; // primary-asset ids
UPROPERTY() FDateTime LastSavedTime;
```
**Update `UMineEconomySubsystem`**
```cpp
UFUNCTION(BlueprintCallable) void SaveGame();   // CreateSaveGameObject -> copy state -> SaveGameToSlot("DeepDelve",0)
UFUNCTION(BlueprintCallable) void LoadGame();   // DoesSaveGameExist -> LoadGameFromSlot -> Cast -> copy -> RebuildWorld()
UFUNCTION(BlueprintCallable) bool HasSave() const;
void RebuildWorld();                            // re-spawn DwarfCount dwarves, set CurrentRock/EquippedPickaxe, broadcast OnTechChanged/OnCoinsChanged
```
Wire `UMenuWidget::Continue` (enabled only if `HasSave()`); `NewGame` calls `ResetToDefaults` **and** `UGameplayStatics::DeleteGameInSlot`. Autosave on a repeating timer.

### 🔵 Blueprints
- None new (the `USaveGame` subclass is used directly from C++). Wire the menu buttons' events to the C++ `UFUNCTION`s.

### 🎛️ UPROPERTY → values
*(none — save state is code; slot name is a constant)*

🏁 **Milestone:** relaunch → Continue → dwarves + depth restored. 🧪 **Boss:** delete the save; a clean first-run still works. 🎁 **Reward:** Master Smith · +300 XP.
💡 *Gotcha:* bump `SaveVersion` and default missing fields on load — later quests add fields and old saves must not crash.

---

# QUEST 11 — Ship a Demo · ~45 min

**Goal.** Package the C++ build and send it to a friend.

🎮 **After:** a standalone Deep Delve demo a friend can run with no Unreal installed.

### 🧩 C++ work
- None new. (Because this is a C++ project, packaging now requires the toolchain — but the packaged build is self-contained; recipients need nothing.)

### Steps
1. Confirm Game Default Map = MainMenu, project name/icon set.
2. **Platforms → Windows → Package Project**, config **Development**.
3. Test the packaged `.exe`: new game → upgrade → save → quit → relaunch → Continue.
4. Zip + share (Drive/WeTransfer).

🏁 **Milestone:** a shippable demo. 🧪 **Boss:** run it on a clean folder/machine, full cycle. 🎁 **Reward:** Master Smith · +250 XP · 🎉
💡 *Tune the economy* (Cost/CostGrowth on the tech nodes) using your friend's feedback before adding content.

---

# QUEST 12 — Pickaxe Upgrades: Auto-Swing & Crits · ~55 min

**Goal.** New `UTechEffect` subclasses that add an auto-swing and critical hits.

🎮 **After:** the pickaxe swings on its own and occasionally lands a gold "CRIT!".

### 🧩 C++ classes & methods
**New `UTechEffect` subclasses:** `UTechEffect_UnlockAutoSwing`, `UTechEffect_UpgradeSwingSpeed`, `UTechEffect_UnlockCrit`, `UTechEffect_UpgradeCritChance` — each writes a field on the subsystem.
**Update `UMineEconomySubsystem`:** `bool bAutoSwing; float AutoSwingInterval=1.f; float CritChance=0; float CritMultiplier=5.f;` + an auto-swing timer that damages `CurrentRockActor`.
**Update `ADeepDelvePlayerController::OnDig` (and auto-swing):** roll `FMath::FRand() < CritChance` → multiply damage by `CritMultiplier`; play crit sound (`UPROPERTY TObjectPtr<USoundBase> CritSound`); spawn a crit-styled damage number.

### 🔵 Blueprints / Data
- `DA_Node_AutoSwing`, `DA_Node_SwingSpeed`, `DA_Node_CritStrike`, `DA_Node_CritChance` (`UTechNode`s). `WBP_DamageNumber` crit variant.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `DA_Node_SwingSpeed` | `Effect` (repeatable) | `UTechEffect_UpgradeSwingSpeed` (e.g. −0.1s/level) |
| `DA_Node_CritChance` | `Effect` (repeatable) | `UTechEffect_UpgradeCritChance` (+5%/level) |
| `BP_MineController` | `CritSound` | `S_Crit` |

🏁 **Milestone:** hands-free swings + flashy crits. 🧪 **Boss:** `CritChance = 1.0` → every hit crits. 🎁 **Reward:** Deeplord · +300 XP.

---

# QUEST 13 — The Growing Crew · ~50 min

**Goal.** Global crew multipliers that update live dwarves.

🎮 **After:** buy a bigger, harder-hitting, faster crew; existing dwarves speed up at once.

### 🧩 C++ classes & methods
**New effects:** `UTechEffect_UpgradeCrewDamage`, `UTechEffect_UpgradeCrewSpeed` → `Econ->CrewDamageMult`/`CrewSpeedMult`.
**Update `UMineEconomySubsystem`:** `float CrewSpeedMult=1.f;` + `DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnCrewStatsChanged) OnCrewStatsChanged;` broadcast on change.
**Update `ADwarf`:** bind `Econ->OnCrewStatsChanged` in BeginPlay → recompute damage and **reset `SwingTimer`** with the new interval (`MinerData->Speed / CrewSpeedMult`).

### 🔵 Data
- `DA_Node_CrewDamage`, `DA_Node_CrewSpeed` (`UTechNode`s). Optional new `UMinerData` dwarf tiers + hire nodes.

🏁 **Milestone:** upgrades ramp all dwarves live. 🧪 **Boss:** buy Crew Speed twice with several dwarves; all speed up. 🎁 **Reward:** Deeplord · +300 XP.

---

# QUEST 14 — Rich Veins · ~55 min

**Goal.** Yield upgrade, new vein tiers, and a lucky mineral drop.

🎮 **After:** richer tiers as you descend, multiplied payouts, and occasional jackpot gems.

### 🧩 C++ classes & methods
**New effect:** `UTechEffect_UpgradeYield` → `Econ->CoinMult += Amount`.
**`AMineral : ACoin`** (inherits `Collect()`; higher `Value`, gem mesh) — or just a `BP_Mineral` subclass of `ACoin`.
**Update `AOreVein`:** apply `Econ->CoinMult` to coin count; add `UPROPERTY float MineralChance=0.02f; int32 MineralValue=100; TSubclassOf<ACoin> MineralClass;` → on break, roll `MineralChance` → spawn a mineral. Add more `URockData` tiers + `UTechEffect_Descend` nodes.

### 🔵 Blueprints / Data
- **`BP_Mineral`** (parent `ACoin`). New `DA_` rock tiers (Silver/Gold/Mithril), `DA_Node_Yield`, `DA_Node_Descend_*`.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_Mineral` | `Mesh` / `Value` | Synty gem mesh / `100` |
| `BP_OreVein` | `MineralClass` / `MineralChance` | `BP_Mineral` / `0.02` |

🏁 **Milestone:** richer tiers + jackpot gems. 🧪 **Boss:** `MineralChance = 1.0`, descend a couple tiers, confirm deeper pays more. 🎁 **Reward:** Deeplord · +350 XP.
💡 *Balance:* deeper tiers should raise `CoinReward` faster than `MaxHP`.

---

# QUEST 15 — The Coin Collector · ~55 min

**Goal.** A walking dwarf that auto-collects coins/minerals by overlap.

🎮 **After:** a dwarf strolls across the mine hoovering up coins on its own.

### 🧩 C++ classes & methods
**`ACollectorDwarf : AActor`**
```cpp
UPROPERTY(VisibleAnywhere) TObjectPtr<USkeletalMeshComponent> Mesh;
UPROPERTY(VisibleAnywhere) TObjectPtr<class USphereComponent> Reach; // Generate Overlap Events = true
UPROPERTY(EditAnywhere) float Speed = 200.f;
UPROPERTY(EditAnywhere) float LeftX, RightX; // sweep bounds
virtual void Tick(float Dt) override;        // move left by Speed*Dt (× Econ->CollectorSpeedMult); wrap at LeftX
UFUNCTION() void OnReachOverlap(...);        // if Cast<ACoin>(Other) -> Coin->Collect();
```
**New effects:** `UTechEffect_HireCollector`, `UTechEffect_UpgradeCollectorSpeed`. **Update subsystem:** `float CollectorSpeedMult=1.f;` + spawn collectors.
Make `ACoin::Collect()` **public** (already) so overlap can call it.

### 🔵 Blueprints / Data
- **`BP_Collector`** (parent `ACollectorDwarf`), `DA_Node_HireCollector`, `DA_Node_CollectorSpeed`.

### 🎛️ UPROPERTY → values
| Asset | Field | Value |
|---|---|---|
| `BP_Collector` | `Mesh` / walk anim / `Speed` | Synty dwarf / walk anim / `200` |
| `BP_Collector` | `Reach` | Sphere, **Generate Overlap Events = true**, overlaps coins |

🏁 **Milestone:** the collector auto-sweeps coins. 🧪 **Boss:** pile coins; confirm it sweeps them; buy a second collector. 🎁 **Reward:** Deeplord · +300 XP.
💡 *Gotcha:* overlap needs **Generate Overlap Events = true on BOTH** the sphere and the coin, with responses set to overlap.

---

# QUEST 16 — Strike It Rich · ~60 min · *finale*

**Goal.** Juice (animation, Niagara, Sound Cues) + a polished **Shipping** v1.0.

🎮 **After:** a polished standalone v1.0 — clicks punch, coins ching, crits boom, dust and sparks fly.

### 🧩 C++ classes & methods
- Add `UPROPERTY(EditAnywhere) TObjectPtr<class UNiagaraSystem> BreakVFX;` to `AOreVein` (and dust/spark VFX to crits, mineral shimmer, coin poof); spawn via `UNiagaraFunctionLibrary::SpawnSystemAtLocation`. Add `"Niagara"` to `Build.cs`.
- Optionally convert the raw `Play Sound 2D` calls to **Sound Cues** (still `USoundBase` UPROPERTYs — just assign a `SoundCue` instead of a `SoundWave`).

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

🏁 **Milestone:** a polished, playable v1.0. 🧪 **Boss:** the Shipping build plays the whole game with sound + effects. 🎁 **Reward:** **Deeplord, Shaper of the Deep** · +400 XP · 🏅 QUEST LINE COMPLETE.

**🎓 What to learn next:** Gameplay Ability System (GAS), replication & multiplayer, and AI / Behavior Trees.

---

*Engine: Unreal Engine 5.8 · IDE: Rider · Art: Synty POLYGON Dungeon Realms · Docs: dev.epicgames.com. This is a first migration pass of the C++-first chain — to be refined next.*
