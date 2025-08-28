# AMX Mod X - Context7 Documentation

## Overview
AMX Mod X is a Metamod plugin for Half-Life 1 that provides comprehensive scripting capabilities for the game engine and its mods. It allows developers to create plugins that can intercept network messages, log events, handle commands, modify entities, and extend functionality through modules.

## Key Features
- **Scripting Engine**: Pawn-based scripting language for Half-Life 1
- **Module System**: Extensible architecture supporting MySQL, Sockets, and other modules
- **Event Interception**: Ability to intercept and modify game events
- **Command Handling**: Custom command and client command processing
- **Entity Modification**: Direct access to game entities and properties
- **Logging System**: Comprehensive event logging capabilities

## Repository Structure
```
amxmodx/
├── amxmodx/          # Core AMX Mod X source code
├── compiler/         # Pawn compiler
├── configs/          # Configuration files
├── modules/          # Extension modules
├── plugins/          # Example plugins
├── public/           # Public headers and includes
└── tools/            # Development tools
```

## Core Components

### 1. Core Engine (amxmodx/)
- Main plugin functionality
- Pawn virtual machine integration
- Metamod plugin interface
- Event handling system

### 2. Modules System (modules/)
- **MySQL Module**: Database connectivity
- **Socket Module**: Network communication
- **Fakemeta Module**: Entity manipulation
- **Fun Module**: Utility functions
- **Engine Module**: Engine-specific functions

### 3. Include Files (public/)
- **amxmodx.inc**: Core functions and natives
- **fakemeta.inc**: Entity manipulation
- **fun.inc**: Utility functions
- **engine.inc**: Engine functions
- **sqlx.inc**: Database operations

## Development Workflow

### 1. Plugin Structure
```pawn
#include <amxmodx>
#include <fakemeta>
#include <fun>

public plugin_init() {
    register_plugin("Plugin Name", "1.0", "Author")
    // Plugin initialization
}

public plugin_cfg() {
    // Configuration loading
}
```

### 2. Event Handling
```pawn
public client_putinserver(id) {
    // Player connects
}

public client_disconnect(id) {
    // Player disconnects
}

public client_death(killer, victim, weapon, hitplace) {
    // Player death event
}
```

### 3. Command Registration
```pawn
public plugin_init() {
    register_clcmd("say /command", "HandleCommand")
    register_concmd("admin_command", "AdminCommand", ADMIN_LEVEL, "Description")
}
```

## Key Natives and Functions

### Core Functions
- `register_plugin()`: Register plugin information
- `register_event()`: Register event handlers
- `register_clcmd()`: Register client commands
- `register_concmd()`: Register console commands
- `get_user_*()`: Get player information
- `set_user_*()`: Set player properties

### Entity Functions
- `entity_get_*()`: Get entity properties
- `entity_set_*()`: Set entity properties
- `find_ent_by_*()`: Find entities
- `create_entity()`: Create new entities

### Database Functions
- `SQL_Connect()`: Connect to database
- `SQL_Query()`: Execute queries
- `SQL_ReadResult()`: Read query results
- `SQL_FreeHandle()`: Free database handles

## Module Development

### Creating Custom Modules
```cpp
#include <amxmodx>
#include <amxmisc>

// Native function registration
static cell AMX_NATIVE_CALL my_native(AMX *amx, cell *params) {
    // Native function implementation
    return 1;
}

// Plugin natives
AMX_NATIVE_INFO my_natives[] = {
    {"my_native", my_native},
    {NULL, NULL}
};

// Plugin initialization
void OnAmxxAttach() {
    MF_AddNatives(my_natives);
}
```

## Best Practices

### 1. Memory Management
- Always free handles (SQL, file, etc.)
- Use proper variable scoping
- Avoid memory leaks in loops

### 2. Error Handling
- Check return values from functions
- Validate user input
- Handle database connection errors

### 3. Performance
- Use efficient loops and algorithms
- Minimize database queries
- Cache frequently used data

### 4. Security
- Validate all user input
- Use SQL prepared statements
- Sanitize output data

## Common Use Cases

### 1. Player Management
```pawn
public client_putinserver(id) {
    new name[32], authid[32]
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    
    // Load player data from database
    load_player_data(id, name, authid)
}
```

### 2. Event Logging
```pawn
public client_death(killer, victim, weapon, hitplace) {
    new killer_name[32], victim_name[32]
    get_user_name(killer, killer_name, charsmax(killer_name))
    get_user_name(victim, victim_name, charsmax(victim_name))
    
    log_amx("Player %s killed %s with %s", killer_name, victim_name, weapon)
}
```

### 3. Database Operations
```pawn
public load_player_data(id, const name[], const authid[]) {
    new query[256]
    formatex(query, charsmax(query), "SELECT * FROM players WHERE authid='%s'", authid)
    
    SQL_ThreadQuery(g_sqlTuple, "HandlePlayerData", query, "", 0)
}
```

## Integration with Half-Life 1

### Game Events
- `client_putinserver`: Player connects
- `client_disconnect`: Player disconnects
- `client_death`: Player death
- `client_spawn`: Player spawns
- `client_team`: Player changes team

### Game Commands
- `say`: Chat messages
- `say_team`: Team chat
- `kill`: Suicide command
- `jointeam`: Team joining
- `spectate`: Spectator mode

## Debugging and Testing

### 1. Logging
```pawn
log_amx("Debug: %s", debug_message)
server_print("Server: %s", message)
client_print(id, print_chat, "Chat: %s", message)
```

### 2. Error Checking
```pawn
if (!is_user_alive(id)) {
    return PLUGIN_HANDLED
}

if (!is_user_connected(id)) {
    return PLUGIN_CONTINUE
}
```

## Resources and Documentation

### Official Resources
- **Website**: https://www.amxmodx.org/
- **Documentation**: https://www.amxmodx.org/doc/
- **API Reference**: https://www.amxmodx.org/api/
- **Forums**: https://forums.alliedmods.net/

### Development Tools
- **AMX Mod X Studio**: IDE for plugin development
- **Pawn Compiler**: Command-line compiler
- **Plugin Validator**: Syntax and logic checking

### Community
- **AlliedModders Forums**: Main community hub
- **Plugin Repository**: Shared plugins and examples
- **Tutorial Section**: Learning resources

## Version Information
- **Current Version**: 1.10.0
- **Supported Games**: Half-Life 1, Counter-Strike 1.6, Day of Defeat, Team Fortress Classic
- **License**: GPL v2
- **Development Status**: Active maintenance

## Context7 Integration Notes
This documentation provides comprehensive coverage of AMX Mod X for Context7 integration, including:
- Core functionality and architecture
- Development patterns and best practices
- Common use cases and examples
- Integration with Half-Life 1 engine
- Debugging and testing approaches
- Community resources and support

The information is structured to support developers working with AMX Mod X plugin development, providing both high-level overview and specific implementation details.
