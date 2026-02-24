# SquadLink UI Feature Reference

This document outlines the features of the SquadLink user interface. The UI is composed of several independent panels for monitoring and manipulating the game world in real-time.

## 1. Core UI Concepts

*   **Object Inspection**: The primary workflow involves selecting game objects (like players or vehicles) from an outliner panel and viewing their detailed properties in the main "Object Inspector" window.
*   **Docking System**: The UI is comprised of multiple panels that can be moved, resized, and docked into a custom layout.
*   **Real-Time Interaction**: Modifications made through the UI, such as changing an actor's position or a team's tickets, are executed in the game in real-time.

---

## 2. Main Panels

These are the primary, persistent windows that provide the core functionality of the tool.

### World Outliner
A hierarchical browser for all actors currently in the game world.
*   **Refresh**: Manually updates the list of actors.
*   **Filter**: A text box to filter the actor list by name.
*   **Categorization**: Actors are grouped into collapsible headers by their class name (e.g., `ASQVehicle`, `ASQSoldier`).
*   **Iconography**: Each actor and category has an icon for quick identification.
*   **Multi-Select**: Supports selecting multiple actors using `Ctrl+Click`.
*   **Context Menu**: Right-clicking an actor provides options to:
    *   **Focus**: Moves the 3D viewport camera to the actor.
    *   **Delete**: Destroys the actor in the world.
    *   **Copy Class Names**: Copies the class names of all selected objects to the clipboard.

### Player Outliner
A dedicated panel listing all players currently connected to the server.
*   **Player List**: Displays a selectable list of all players by name.
*   **Object Inspection**: Selecting a player populates the Object Inspector with their details.
*   **Context Menu**: Right-clicking a player allows you to:
    *   **Teleport Viewport to Player**: Moves the 3D viewport camera to the selected player's position.

### Script Editor
A tab-based editor for creating and modifying Lua scripts.
*   **Multi-File Editing**: Open and edit multiple scripts, each in its own tab.
*   **Unsaved Changes Indicator**: Tabs are marked with an asterisk (`*`) to indicate unsaved changes.
*   **Code Input**: A multi-line text field with support for tab indentation.

### Script Browser
A simple file browser for locating and opening Lua scripts.
*   **File Navigation**: Lists all `.lua` files in the designated scripts directory.
*   **Open Script**: Double-clicking a file opens it in the Script Editor.

### Lua Executor
An immediate-execution console for running Lua code snippets.
*   **Code Input**: A multi-line text box for entering code.
*   **Execution**: An "Execute" button runs the code in the game's Lua environment.
*   **Result Display**: A read-only text box shows any output or errors from the executed code.

### Process Event Inspector
A powerful debugging tool for monitoring internal game engine function calls.
*   **Function List**: Displays a list of all captured engine functions.
*   **Filtering**: A text box to filter the function list.
*   **Parameter View**: Selecting a function shows its parameter structure (name, type, size, offset).
*   **Event Log**: A table displays the most recent calls for the selected function, including a timestamp and the value of each parameter for that call.

### Console Log
Displays the application's log output.
*   **Log Viewer**: Shows the contents of the log file.
*   **Verbose Toggle**: A checkbox to show or hide detailed verbose-level logs.

### Other Panels
The code also references the existence of the following panels:
*   **3D Viewport**: A render of the 3D game world with a controllable camera.
*   **Object Inspector**: The host panel that contains the various "Inspector Panels" detailed below.
*   **Profiler**: A panel for displaying performance metrics.
*   **Settings**: A panel for tool configuration.

---

## 3. Inspector Panels

These panels appear within the main "Object Inspector" window and display context-sensitive information based on the type of object selected in an outliner.

### Actor Inspector
Displays properties for any basic `Actor`.
*   **Properties**:
    *   `Location`, `Rotation`, `Scale`: View and edit the actor's transform with draggable float inputs.
    *   `Mobility`: Change the actor's mobility between Static, Stationary, and Movable via a dropdown.
*   **Actions**:
    *   **Teleport To**: Moves the 3D viewport camera to the actor.
    *   **Show/Hide from Viewport**: Toggles the actor's visibility in the game.

### Soldier Inspector
Displays properties for a `Soldier` (player character).
*   **Properties**:
    *   `Health`: A progress bar showing current health out of 100.
    *   `IsSprinting`: Shows if the soldier is currently sprinting.
    *   `IsWounded`: Shows if the soldier is in the wounded (downed) state.
*   **Actions**:
    *   **Heal**: A button to restore the soldier to full health.

### Vehicle Inspector
Displays properties for a `Vehicle`.
*   **Properties**:
    *   `Health`: A progress bar showing current health relative to max health.
    *   `Speed`: Displays the vehicle's current speed in km/h.
    *   `Claimed by`: Shows which squad has claimed the vehicle.
    *   `Seats`: A collapsible list showing each seat and the name of the player occupying it.
*   **Actions**:
    *   **Repair**: Restores the vehicle to full health.
    *   **Destroy**: Sets the vehicle on fire, leading to its destruction.
    *   **Eject All**: Ejects all players from the vehicle.

### Inventory Inspector
Manages a soldier's `Inventory`.
*   **Item List**: A table showing all items with their `Slot`, `Offset`, and `Class`.
*   **Item Actions**:
    *   **Switch To**: Forces the soldier to equip the selected item.
    *   **Remove**: Deletes the item from the inventory.
*   **Add Item Popup**:
    *   A searchable, selectable list of all available items in the game.
    *   Allows specifying a target `Slot` and `Offset` to add the new item.

### Player Controller Inspector
Shows data from the `PlayerController`, the object representing the player's connection.
*   **Properties**: Displays a detailed list of state variables, including `TeamId`, `SquadId`, `IsIncapacitated`, `SpawnDelayPenalties`, and `LastDeathTime`.
*   **Actions**:
    *   **Force Respawn**: Forces the player to respawn.

### Player State Inspector
Shows scoreboard and state information from the `PlayerState`.
*   **Properties**: Displays `Player Name`, `Team ID`, `Squad ID`, `Score`, `Kills`, `Deaths`, and status flags like `Is Admin` and `Is Alive`.

### Game State Inspector
Manages the overall `GameState` for the current match.
*   **Properties**: Displays `Server Name`, `Map Name`, `Game Mode`, `PlayerCount`, and `Time Remaining`.
*   **Actions**:
    *   **Pause Timer**: A checkbox to pause or resume the match timer.
    *   **Set Remaining Time**: An input field and button to change the match time.

### Team State Inspector
Manages data for a specific `TeamState`.
*   **Properties**: Displays `Team ID`, `Faction`, `Tickets`, `Score`, `PlayerCount`, and `SquadCount`.
*   **Actions**:
    *   **Adjust Tickets**: An input field and button to add or remove tickets from the team.

### Squad State Inspector
Manages data for a specific `SquadState`.
*   **Properties**: Displays `Squad ID`, `Squad Name`, `Team ID`, `PlayerCount`, and `LeaderName`.
*   **Actions**:
    *   **Locked**: A checkbox to view and change the squad's locked status.

### Spawner Inspector
Manages properties of a `Spawner` (e.g., a FOB radio).
*   **Properties**:
    *   `Enabled`: A checkbox to enable or disable the spawn point.
    *   `Team`: An input to view and change the spawner's team allegiance.
    *   Displays status flags like `IsSpawnInProgress` and `IsSpawnOverlapped`.
*   **Actions**:
    *   **Try Spawn**: Attempts to trigger a spawn.
    *   **Reset Delay**: Resets the spawn timer.
    *   **Init Vehicle**: (For vehicle spawners) Initializes the vehicle setting.

### Weapon Inspector
Displays the state of a soldier's currently equipped `Weapon`.
*   **Properties**: Shows status flags like `bIsFiring` and `bIsReloading`, `CurrentFireMode`, and `ZoomedFOV`.
*   **Magazines**: A collapsible list showing the ammo count for each magazine.

### Command-Related Inspectors
*   **Commander State**: Shows the status of a team's commander, including the current commander's name and voting status.
*   **Command Zone**: For commander-specific areas, showing if actions are allowed and listing players inside the zone.

### Resource Inspectors
*   **Vehicle Resource**: Used for supply vehicles, it shows current `Resources` on a progress bar and has a button to `Refill Resources`.
