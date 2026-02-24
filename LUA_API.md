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
        *   **Character** (Bipedal Pawn)
            *   **Soldier** (Player-controlled human)
    *   **Controller** (The "Brain")
        *   **PlayerController**

---

## 2. Managers

Managers are singletons accessed via `SquadLink`.

### `SquadLink` (Global Table)
*   `GetPlayerManager()`: Returns `PlayerManager`.
*   `GetServerManager()`: Returns `ServerManager`.
*   `GetGameStateManager()`: Returns `GameStateManager`.
*   `GetWorldManager()`: Returns `WorldManager`.
*   `GetEvents()`: Returns `Events`.
*   `LoadAsset(path)`: Loads a UClass (Blueprint). **Note:** Append `_C` to the end of the path.

### `Events`
Used to register callbacks for game actions.
*   `OnTick(callback)`: Fires every 100ms.
*   `OnPlayerJoin(callback)`: Returns `Player`.
*   `OnPlayerLeave(callback)`: Returns `Player`.
*   `OnChatMessage(callback)`: Returns `Player`, `string`.
*   `OnPlayerSpawn(callback)`: Returns `Player`.
*   `OnBlueprintPlayerDied(callback)`: Returns `Player`.
*   `OnBPDie(callback)`: Returns `Victim` (Soldier), `Params` (Killer/Causer).
*   `OnPlayerCommendationData(callback)`: Returns `Player`, `Data`.

### `PlayerManager`
*   `GetPlayers()`: Returns a table of all active `Player` objects.
*   `GetPlayerByName(name)`: Returns `Player` or `nil`.
*   `GetPlayerByUniqueId(uid)`: Returns `Player` or `nil` (uses EOS ID).

### `ServerManager`
*   `SendMessageToAll(message, color, type)`: Sends a system broadcast.
*   `GetGameMode()`: Returns `GameMode` wrapper.

### `WorldManager`
*   `GetActors()`: Table of all `Actor` wrappers.
*   `GetAllGameSpawns()`: Table of all `GameSpawn` objects.
*   `GetAllVehicles()`: Table of all `Vehicle` objects.
*   `FindClassByName(name)`: Returns `UClass` or `nil`.
*   `SpawnActor(class, location, rotation)`: Spawns an object into the world.

---

## 3. State Objects

These objects manage the "Meta" data of the match (tickets, squads, faction info).

### `GameState`
*   **Getters:** `IsTimerPaused()`, `GetMatchID()`, `GetMapName()`, `GetPlayerCount()`, `GetRemainingTime()`, `GetServerTickRate()`, `GetTeamState(id)`.
*   **Setters:** `SetTimerPaused(bool)`, `SetServerName(string)`, `SetMessageOfTheDay(string)`, `SetRemainingTime(seconds)`.

### `TeamState`
*   **Getters:** `GetTickets()`, `GetId()`, `GetNumKills()`, `GetNumDeaths()`, `GetFactionDisplayName()`, `GetSquadStates()`.
*   **Functions:** `SetTickets(value)`, `AdjustTickets(delta, reason)`, `ScorePoints(points, reason)`.

### `SquadState`
*   **Getters:** `GetName()`, `GetId()`, `GetTeamId()`, `GetPlayerCount()`, `IsLocked()`, `GetLeaderState()`.
*   **Functions:** `ScorePoints(points, reason)`.

### `PlayerState`
*   **Getters:** `GetTeamId()`, `GetOnlineUserId()`, `GetSoldier()`, `GetCurrentRole()`, `IsAdmin()`, `IsAlive()`, `GetShortPlayerName()`.
*   **Setters:** `SetTeamId(id)`, `SetPlayerName(name)`, `SetIsAdmin(bool)`.

---

## 4. World Objects (Actors)

### `Actor`
*   `GetName()`, `GetClass()`, `Destroy()`.
*   `GetActorLocation()`, `SetActorLocation(vector)`.
*   `GetActorRotation()`, `SetActorRotation(rotator)`.

### `Soldier` (Inherits Character -> Pawn -> Actor)
*   **Health:** `GetHealth()`, `SetHealth(val)`, `Heal(amount)`, `IsWounded()`, `IsBleeding()`, `Revive()`, `Suicide()`.
*   **Movement:** `IsSprinting()`, `Prone()`, `UnProne()`, `Jump()`, `LaunchCharacter(velocity)`.
*   **Weapons:** `SwitchWeapon(slot)`, `GetInventory()`, `BeginFireWeapon()`, `EndFireWeapon()`, `ServerLowerWeapon(bool)`.

### `Vehicle` (Inherits Pawn -> Actor)
*   `GetHealth()`, `SetHealth(val)`, `IsDestroyed()`.
*   `EjectAllPlayers()`, `Repair(amount)`, `Burn(rate, soldierRate, causer)`.
*   `IsEmpty()`, `IsFull()`, `GetNumOccupants()`, `GetClaimingSquad()`.

---

## 5. Player Connection (`Player`)

The `Player` object represents the connection and high-level controller.

*   **Identity:** `GetName()`, `GetUniqueId()`, `IsAdmin()`, `GetTeam()`, `GetSquad()`.
*   **State:** `IsAlive()`, `IsWounded()`, `IsInVehicle()`, `GetSoldier()`.
*   **Communcation:** `SendMessage(msg, color, type)`, `ChatToAll(msg)`, `ChatToSquad(msg)`.
*   **Management:** `ChangeTeamById(id)`, `CreateSquad(name, locked)`, `KickFromSquad(player)`, `ChangeRole(role)`.
*   **Respawn:** `Respawn()`, `GiveUp()`, `SetSpawnDelayPenalties(val)`, `ServerRequestRestartPlayer(spawnObject)`.

---

## 6. Spawning System

### `GameSpawn`
*   **Getters:** `IsSpawningEnabled()`, `IsSieged()`, `GetTeam()`, `GetSpawnPointType()`, `CanSpawn()`.
*   **Setters:** `SetSpawningEnabled(bool)`, `SetTeam(teamId)`, `SetRespawnDelay(seconds)`.
*   **Core:** `GetSdkObject()` (Required for `ServerRequestRestartPlayer`).

### `TeamSpawnGroup`
*   Inherits all `GameSpawn` functions.
*   `GetSpawnPoints()`: Returns table of `TeamSpawnPoint`.

---

## 7. Data Types & Enums

### Types
*   `FVector(x, y, z)`
*   `FRotator(p, y, r)`
*   `FLinearColor(r, g, b, a)`
*   `FString` (Auto-converts to Lua string via `tostring()`)

### Enums
*   **ESQNotificationTypes:** `Message`, `Warning`, `Error`, `Positive`, `Negative`, `AdminNotification`.
*   **ESQTeam:** `Team1`, `Team2`.
*   **ESQSpawnPointType:** `Main`, `FOB`, `Rally`, `Vehicle`.

---

## 8. Full Example Script

```lua
local Events = SquadLink.GetEvents()
local Server = SquadLink.GetServerManager()

-- Broadcast join messages
Events:OnPlayerJoin(function(player)
    local name = player:GetName()
    local green = FLinearColor(0, 1, 0, 1)
    Server:SendMessageToAll(name .. " has joined.", green, ESQNotificationTypes.Positive)
end)

-- Basic Admin Commands
Events:OnChatMessage(function(player, message)
    if message == "!pos" then
        local soldier = player:GetSoldier()
        if soldier then
            local loc = soldier:GetActorLocation()
            player:SendMessage("Your Pos: " .. tostring(loc), nil, ESQNotificationTypes.Message)
        end
    end
    
    -- Admin only: Kill all vehicles
    if message == "!clearvehicles" and player:IsAdmin() then
        local vehicles = SquadLink.GetWorldManager():GetAllVehicles()
        for _, v in ipairs(vehicles) do
            v:Destroy()
        end
        player:SendMessage("Vehicles cleared.", nil, ESQNotificationTypes.Warning)
    end
end)
```