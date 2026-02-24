# SquadLink Documentation

SquadLink is a server-side runtime for Squad that executes Lua scripts directly on your server. It is designed to extend game functionality, create custom gamemodes, and build admin tools without ever touching the official SDK. Because all logic is handled on the backend, players join with a standard vanilla client and never have to download workshop files.

### Development without the SDK
You can modify game behavior using standard Lua 5.4. This removes the need to navigate the Unreal Editor or handle the heavy repacking process. It is a lightweight alternative for developers who want to focus on logic rather than assets.

### Seamless Player Experience
Since the environment is purely server-side, your server remains "vanilla-friendly." Players don't face extra download screens, making it ideal for community events or competitive leagues that require custom rules on standard maps.

### Live Environment
The system supports live-reloading. You can edit your scripts and apply changes while the server is active, allowing for rapid testing and iteration without the need for constant restarts.

## Resources

* **[Lua API Reference](LUA_API.md)**
  A full technical breakdown of every server-side function available in the environment.

* **[Example Scripts](https://github.com/SquadExtended/community-scripts)**
  A collection of community-contributed logic, ranging from simple admin tools to complex gamemodes.

---
*Note: SquadLink is an unofficial project and is not affiliated with Offworld Industries.*