# SquadLink Lua API Reference

Welcome to the SquadLink scripting reference. This document covers the managers, objects, and events available for building server-side logic in Squad.

## 1. Core Concepts

### The Global Table
Everything in the environment is accessed through the `SquadLink` global table. This is how you access the various managers (Player, Server, World, etc.).

### Inheritance
Many objects in this API follow an inheritance structure. This means a **Soldier** has all the functions of a **Character**, which has all the functions of a **Pawn**, and so on.

**Hierarchy:**
*   **Actor** (Basic world object)
    *   **Pawn** (An Actor that can be possessed)
        *   **Vehicle**
            *   **GroundVehicle**
        *   **Character** (Bipedal Pawn)
            *   **Soldier** (Player-controlled human)
        *   **DefaultPawn** (Simple free-flying/debug pawn)
    *   **Controller** (The "Brain")
        *   **PlayerController**
    *   **Weapon** (Actor that can fire)
    *   **Spawner** (Actor that handles spawning)
        *   **VehicleSpawner**

---

## 2. Global API (`SquadLink`)

Managers are singletons accessed via `SquadLink`.

*   `GetPlayerManager()`: Returns `PlayerManager`.
*   `GetServerManager()`: Returns `ServerManager`.
*   `GetGameStateManager()`: Returns `GameStateManager`.
*   `GetWorldManager()`: Returns `WorldManager`.
*   `GetEvents()`: Returns `Events`.
*   `CreateRconServer()`: Creates an `RconServer` instance.
*   `LoadAsset(path)`: Loads a `UClass` (Blueprint). **Note:** Append `_C` to the end of the path.
*   `LoadCppGameMode(modeName, spawnPoints, boundary)`: Loads a built-in C++ game mode (e.g., "GunGame").

---

## 3. Managers

### `Events`
Used to register callbacks for game actions.
*   `OnTick(callback)`: Fires every 100ms.
*   `OnPlayerJoin(callback)`: Returns `Player`.
*   `OnPlayerLeave(callback)`: Returns `Player`.
*   `OnChatMessage(callback)`: Returns `Player`, `string`, `ESQChat`.
*   `OnPlayerSpawn(callback)`: Returns `Player`.
*   `OnBlueprintPlayerDied(callback)`: Returns `Player`.
*   `OnBPDie(callback)`: Returns `Victim` (Soldier), `Params` (Killer/Causer).
*   `OnPlayerCommendationData(callback)`: Returns `Player`, `Data`.
*   `OnWeaponFire(callback)`: Returns `Weapon`.
*   `OnVehicleEngineStateChanged(callback)`: Returns `GroundVehicle`, `bool` (IsOn).
*   `OnVehicleSeatInteract(callback)`: Returns `Player`, `VehicleSeat`.
*   `OnFlagCaptured(callback)`: Returns `FlagActor`, `TeamId`.
*   `OnPlayerHealed(callback)`: Returns `Healer` (Player), `Healed` (Player), `Amount`.
*   `OnPlayerRevived(callback)`: Returns `Reviver` (Player), `Revived` (Player).
*   `OnExplosivePlaced(callback)`: Returns `Player`, `ExplosiveActor`.
*   `OnCanHearLocalVoicePacket(callback)`: Returns `HeardPlayer` (PlayerController), `HearingPlayer` (PlayerController), `bool` (bIsEnemy).

### `PlayerManager`
*   `GetPlayers()`: Returns a table of all active `Player` objects.
*   `GetPlayerByName(name)`: Returns `Player` or `nil`.
*   `GetPlayerByUniqueId(uid)`: Returns `Player` or `nil` (uses EOS ID).

### `ServerManager`
*   `SendMessageToAll(message, color, type)`: Sends a system broadcast.
*   `GetGameMode()`: Returns `GameMode` wrapper.
*   `AdminTeleport(player, targetLocation)`: Teleports a player.
*   `KickFromQueue(player)`: Kicks a player from the server queue.
*   `ExecuteConsoleCommand(command)`: Executes a server console command.

### `WorldManager`
*   `GetActors()`: Table of all `Actor` wrappers.
*   `GetAllGameSpawns()`: Table of all `GameSpawn` objects.
*   `GetAllTeamSpawnGroups()`: Table of all `TeamSpawnGroup` objects.
*   `GetAllVehicles()`: Table of all `Vehicle` objects.
*   `GetAllSpawners()`: Table of all `Spawner` objects.
*   `GetAllVehicleSpawners()`: Table of all `VehicleSpawner` objects.
*   `FindClassByName(name)`: Returns `UClass` or `nil`.
*   `SpawnActor(class, location, rotation)`: Spawns an object into the world. Returns the appropriate wrapper (Soldier, Vehicle, etc.).

---

## 4. State Objects

These objects manage the "Meta" data of the match (tickets, squads, faction info).

### `GameState`
*   **Getters:** `IsTimerPaused()`, `GetMatchID()`, `GetMapName()`, `GetPlayerCount()`, `GetRemainingTime()`, `GetServerTickRate()`, `GetTeamState(id)`, `GetGameModeName()`, `GetServerName()`, `GetMessageOfTheDay()`, `GetMaxPlayers()`.
*   **Setters:** `SetTimerPaused(bool)`, `SetServerName(string)`, `SetMessageOfTheDay(string)`, `SetRemainingTime(seconds)`, `SetChangeTeamWaitTime(seconds)`.

### `TeamState`
*   **Getters:** `GetTickets()`, `GetId()`, `GetScore()`, `GetNumKills()`, `GetNumDeaths()`, `GetFactionDisplayName()`, `GetSquadStates()`, `GetPlayerStates()`, `GetPlayerCount()`.
*   **Functions:** `SetTickets(value)`, `AdjustTickets(delta, reason)`, `ScorePoints(points, reason)`.

### `SquadState`
*   **Getters:** `GetName()`, `GetId()`, `GetTeamId()`, `GetPlayerCount()`, `IsLocked()`, `GetLeaderState()`, `GetPlayerStates()`.
*   **Functions:** `ScorePoints(points, reason)`.

### `PlayerState`
*   **Getters:** `GetTeamId()`, `GetOnlineUserId()`, `GetSoldier()`, `GetCurrentRole()`, `IsAdmin()`, `IsAlive()`, `IsCommander()`, `IsSquadLeader()`, `GetShortPlayerName()`, `GetFullPlayerName()`, `GetLives()`, `GetNumKills()`, `GetNumDeaths()`.
*   **Setters:** `SetTeamId(id)`, `SetPlayerName(name)`, `SetIsAdmin(bool)`, `SetLives(val)`.

---

## 5. World Objects (Actors)

### `Actor`
*   `GetName()`, `GetClass()`, `Destroy()`.
*   `GetActorLocation()`, `SetActorLocation(vector)`.
*   `GetActorRotation()`, `SetActorRotation(rotator)`.
*   `GetActorForwardVector()`, `GetActorRightVector()`, `GetActorUpVector()`.

### `Pawn` (Inherits Actor)
*   `GetController()`: Returns `Controller`.
*   `IsPlayerControlled()`: Returns `bool`.

### `Character` (Inherits Pawn)
*   `Jump()`, `StopJumping()`, `Crouch()`, `UnCrouch()`, `IsCrouched()`, `LaunchCharacter(velocity)`.

### `Soldier` (Inherits Character)
*   **Health:** `GetHealth()`, `GetMaxHealth()`, `SetHealth(val)`, `Heal(amount)`, `IsWounded()`, `IsDying()`, `IsBleeding()`, `Revive()`, `Suicide()`.
*   **Actions:** `IsSprinting()`, `Prone()`, `UnProne()`, `StartAimDownSights()`, `EndAimDownSights()`, `BeginFireWeapon()`, `EndFireWeapon()`, `ToggleBipod()`.
*   **Inventory:** `SwitchWeapon(slot)`, `GetInventory()`.

### `Vehicle` (Inherits Pawn)
*   `GetHealth()`, `GetMaxHealth()`, `SetHealth(val)`, `IsDestroyed()`, `IsClaimable()`.
*   `EjectAllPlayers()`, `Repair(amount)`, `Burn(rate, soldierRate, causer)`.
*   `IsEmpty()`, `IsFull()`, `GetNumOccupants()`, `GetSeats()`, `GetClaimingSquad()`.

### `GroundVehicle` (Inherits Vehicle)
*   `isEngineOn()`, `setEngineOn(bool)`, `isHandbrakeOn()`, `getForwardSpeed()`, `getEngineRPM()`.
*   `GetPhysics()`, `SetPhysics(VehiclePhysics)`.

### `Weapon` (Inherits Actor)
*   `ServerStartFire()`, `ServerStopFire()`, `ServerStartReload()`.
*   `IsAimingDownSights()`, `IsReloading()`, `IsFiring()`.
*   `GetCurrentFireMode()`, `GetMagazines()`.

---

## 6. Player Connection (`Player`)

The `Player` object represents the connection and high-level controller.

*   **Identity:** `GetName()`, `GetUniqueId()`, `GetSteamId()`, `GetIPAddress()`, `IsAdmin()`, `GetTeam()`, `GetSquad()`.
*   **State:** `IsAlive()`, `IsWounded()`, `IsInVehicle()`, `GetSoldier()`, `GetPawn()`, `GetController()`.
*   **Communication:** `SendSystemMessage(msg, color, type)`, `ChatToAll(msg)`, `ChatToSquad(msg)`, `ChatToTeam(msg)`.
*   **Management:** `ForceTeamChange()`, `ChangeTeamById(id)`, `CreateSquad(name, locked)`, `DisbandSquad()`, `KickFromSquad(player)`, `ChangeRole(role)`, `RequestRole(role)`.
*   **Vehicle:** `GetVehicle()`, `GetSeatIndex()`, `ServerSwitchSeat(index)`, `ForceEnterVehicle(vehicle, seat)`.
*   **Respawn:** `Respawn()`, `GiveUp()`, `SelectSpawn(spawn)`, `CommitSpawn()`, `SetSpawnDelayPenalties(val)`, `ServerRequestRestartPlayer(spawnObject)`.

---

## 7. Spawning System

### `Spawner`
*   `IsSpawnerEnabled()`, `SetSpawnerEnabled(bool)`.
*   `GetSpawnPosition()`, `GetTeam()`, `SetTeam(id)`, `TrySpawn(class)`.

### `GameSpawn`
*   **Getters:** `IsSpawningEnabled()`, `IsSieged()`, `GetTeam()`, `GetSpawnPointType()`, `CanSpawn()`, `GetRespawnDelay()`.
*   **Setters:** `SetSpawningEnabled(bool)`, `SetTeam(teamId)`, `SetRespawnDelay(seconds)`.
*   **Core:** `GetSdkObject()` (Required for `ServerRequestRestartPlayer`).

### `TeamSpawnGroup`
*   Inherits all `GameSpawn` functions.
*   `GetSpawnPoints()`: Returns table of `TeamSpawnPoint`.

---

## 8. Factions, Roles & Inventory

### `Faction`
*   `GetName()`, `GetKey()`.

### `FactionSetup`
*   `GetFactionId()`, `GetFaction()`, `GetDefaultRole()`, `GetRoles()`.

### `RoleSettings`
*   `GetRoleDisplayName()`, `IsMedic()`, `IsSquadLeader()`, `GetInventory()`.

### `Inventory`
*   `InsertItemIntoInventory(class)`, `RemoveItemFromInventory(class)`, `ServerSwitchWeapon(index)`.

---

## 9. RconServer

Exposes an RCON-like interface for external tools.

*   `Start(port, password)`: Starts the server.
*   `Stop()`: Stops the server.
*   `OnCommand(function(command, args))`: Registers a callback for received commands.

---

## 10. Data Types & Enums

### Types
*   `FVector(x, y, z)`
*   `FVector2D(x, y)`
*   `FRotator(p, y, r)`
*   `FLinearColor(r, g, b, a)`
*   `FString` (Auto-converts to Lua string via `tostring()`)
*   `VehiclePhysics`: Table containing `Mass`, `DragCoefficient`, `MaxTorque`, `IdleRPM`, `MaxRPM`, etc.

### Enums
*   **ESQNotificationTypes:** `Message`, `Warning`, `Error`, `Positive`, `Negative`, `AdminNotification`, `Commander`, `Team`, `Squad`, `Fireteam`.
*   **ESQChat:** `All`, `Team`, `Squad`, `AdminBroadcast`, `AdminChat`, `ServerMessage`.
*   **ESQTeamRelationShip:** `SameTeam`, `Friend`, `Neutral`, `Enemy`.
*   **ESeatProgressMenuMode:** `Entering`, `Exiting`, `Switching`.
