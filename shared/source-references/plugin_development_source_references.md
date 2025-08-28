# Plugin Development - Source References and Advanced Details

## AMX Mod X Source References

### Core Source Files
```
amxmodx/
├── amxmodx/                    # Core AMX Mod X engine
│   ├── amxmodx.cpp            # Main plugin loader
│   ├── amxmodx.h              # Core definitions
│   ├── amxmodx_impl.cpp       # Implementation details
│   ├── amxmodx_impl.h         # Implementation headers
│   ├── amxmodx_utils.cpp      # Utility functions
│   └── amxmodx_utils.h        # Utility headers
├── compiler/                   # Pawn compiler
│   ├── amxcomp.c              # Main compiler
│   ├── amxcomp.h              # Compiler headers
│   ├── amxcons.c              # Console functions
│   ├── amxdbg.c               # Debug information
│   ├── amxexec.c              # Execution engine
│   ├── amxfile.c              # File operations
│   ├── amxfloat.c             # Floating point support
│   ├── amxstring.c            # String handling
│   └── amxcore.c              # Core compiler functions
├── modules/                    # AMX Mod X modules
│   ├── fun/                   # Fun module
│   │   ├── fun.cpp            # Fun module implementation
│   │   ├── fun.h              # Fun module headers
│   │   └── fun_natives.cpp    # Fun native functions
│   ├── engine/                # Engine module
│   │   ├── engine.cpp         # Engine module implementation
│   │   ├── engine.h           # Engine module headers
│   │   └── engine_natives.cpp # Engine native functions
│   ├── fakemeta/              # Fakemeta module
│   │   ├── fakemeta.cpp       # Fakemeta implementation
│   │   ├── fakemeta.h         # Fakemeta headers
│   │   └── fakemeta_natives.cpp # Fakemeta natives
│   ├── sqlx/                  # SQL module
│   │   ├── sqlx.cpp           # SQL implementation
│   │   ├── sqlx.h             # SQL headers
│   │   └── sqlx_natives.cpp   # SQL native functions
│   └── sockets/               # Socket module
│       ├── sockets.cpp        # Socket implementation
│       ├── sockets.h          # Socket headers
│       └── sockets_natives.cpp # Socket native functions
└── public/                    # Public headers
    ├── amxmodx.h              # Main AMX Mod X header
    ├── amxmodx_version.h      # Version information
    ├── amxmodx_const.h        # Constants
    ├── amxmodx_natives.h      # Native function declarations
    └── amxmodx_events.h       # Event system headers
```

### Include File Locations
```
addons/amxmodx/
├── scripting/                 # Plugin source files
│   ├── include/               # Include files
│   │   ├── amxmodx.inc        # Core AMX Mod X include
│   │   ├── amxmodx_const.inc  # Constants
│   │   ├── amxmodx_natives.inc # Native functions
│   │   ├── amxmodx_events.inc # Event system
│   │   ├── fun.inc            # Fun module include
│   │   ├── engine.inc         # Engine module include
│   │   ├── fakemeta.inc       # Fakemeta module include
│   │   ├── sqlx.inc           # SQL module include
│   │   ├── sockets.inc        # Socket module include
│   │   ├── hamsandwich.inc    # HamSandwich include
│   │   ├── cstrike.inc        # Counter-Strike specific
│   │   ├── csx.inc            # CSX module include
│   │   └── vector.inc         # Vector utilities
│   ├── examples/              # Example plugins
│   │   ├── basic_plugin.sma   # Basic plugin example
│   │   ├── admin_plugin.sma   # Admin plugin example
│   │   ├── database_plugin.sma # Database plugin example
│   │   └── socket_plugin.sma  # Socket plugin example
│   └── plugins/               # Compiled plugins
│       ├── *.amxx             # Compiled plugin files
│       └── plugins.ini        # Plugin configuration
```

## ReHLDS Source References

### Core Source Files
```
rehlds/
├── rehlds/                    # Core ReHLDS engine
│   ├── engine/                # Engine core
│   │   ├── sv_main.cpp        # Main server logic
│   │   ├── sv_ents.cpp        # Entity system
│   │   ├── sv_phys.cpp        # Physics engine
│   │   ├── sv_world.cpp       # World management
│   │   ├── sv_net.cpp         # Network handling
│   │   ├── sv_user.cpp        # User management
│   │   ├── sv_edict.cpp       # Entity dictionary
│   │   └── sv_move.cpp        # Movement system
│   ├── common/                # Common utilities
│   │   ├── common.cpp         # Common functions
│   │   ├── common.h           # Common headers
│   │   ├── mathlib.cpp        # Math library
│   │   ├── mathlib.h          # Math headers
│   │   ├── vector.cpp         # Vector operations
│   │   ├── vector.h           # Vector headers
│   │   ├── quaternion.cpp     # Quaternion operations
│   │   └── quaternion.h       # Quaternion headers
│   ├── game/                  # Game specific
│   │   ├── game.cpp           # Game logic
│   │   ├── game.h             # Game headers
│   │   ├── game_shared.cpp    # Shared game code
│   │   ├── game_shared.h      # Shared game headers
│   │   ├── cstrike/           # Counter-Strike specific
│   │   │   ├── cstrike.cpp    # CS game logic
│   │   │   ├── cstrike.h      # CS headers
│   │   │   ├── cs_player.cpp  # CS player logic
│   │   │   ├── cs_player.h    # CS player headers
│   │   │   ├── cs_weapons.cpp # CS weapons
│   │   │   └── cs_weapons.h   # CS weapon headers
│   │   └── dod/               # Day of Defeat specific
│   │       ├── dod.cpp        # DoD game logic
│   │       └── dod.h          # DoD headers
│   ├── network/               # Network layer
│   │   ├── net_main.cpp       # Main network logic
│   │   ├── net_main.h         # Network headers
│   │   ├── net_chan.cpp       # Network channels
│   │   ├── net_chan.h         # Channel headers
│   │   ├── net_msg.cpp        # Network messages
│   │   ├── net_msg.h          # Message headers
│   │   ├── net_ws.cpp         # Winsock implementation
│   │   └── net_ws.h           # Winsock headers
│   ├── dlls/                  # DLL management
│   │   ├── dlls.cpp           # DLL loading
│   │   ├── dlls.h             # DLL headers
│   │   ├── metamod.cpp        # Metamod integration
│   │   ├── metamod.h          # Metamod headers
│   │   ├── amxmodx.cpp        # AMX Mod X integration
│   │   └── amxmodx.h          # AMX Mod X headers
│   └── utils/                 # Utilities
│       ├── utils.cpp          # Utility functions
│       ├── utils.h            # Utility headers
│       ├── crc.cpp            # CRC calculations
│       ├── crc.h              # CRC headers
│       ├── md5.cpp            # MD5 hashing
│       ├── md5.h              # MD5 headers
│       ├── sha1.cpp           # SHA1 hashing
│       └── sha1.h             # SHA1 headers
├── msvc/                      # Visual Studio projects
│   ├── ReHLDS.sln             # Solution file
│   ├── ReHLDS.vcxproj         # Project file
│   ├── ReHLDS.vcxproj.filters # Project filters
│   └── ReHLDS.vcxproj.user    # User settings
├── dep/                       # Dependencies
│   ├── bzip2/                 # Bzip2 compression
│   ├── zlib/                  # Zlib compression
│   ├── openssl/               # OpenSSL cryptography
│   └── sqlite/                # SQLite database
└── .github/workflows/         # CI/CD
    ├── build.yml              # Build workflow
    ├── test.yml               # Test workflow
    └── release.yml            # Release workflow
```

### ReHLDS Plugin Integration
```
rehlds/
├── dlls/
│   ├── amxmodx/               # AMX Mod X integration
│   │   ├── amxmodx.cpp        # AMX Mod X loader
│   │   ├── amxmodx.h          # AMX Mod X headers
│   │   ├── amxmodx_natives.cpp # ReHLDS natives
│   │   ├── amxmodx_natives.h  # Native headers
│   │   ├── amxmodx_events.cpp # Event system
│   │   ├── amxmodx_events.h   # Event headers
│   │   ├── amxmodx_utils.cpp  # Utility functions
│   │   └── amxmodx_utils.h    # Utility headers
│   └── metamod/               # Metamod integration
│       ├── metamod.cpp        # Metamod loader
│       ├── metamod.h          # Metamod headers
│       ├── metamod_plugins.cpp # Plugin management
│       └── metamod_plugins.h  # Plugin headers
```

## Advanced Plugin Development Details

### AMX Mod X Plugin Structure Deep Dive

#### 1. Plugin Entry Points
```cpp
// amxmodx.cpp - Plugin loading mechanism
AMX_NATIVE_INFO plugin_natives[] = {
    {"register_plugin", register_plugin},
    {"register_event", register_event},
    {"register_forward", register_forward},
    {"register_clcmd", register_clcmd},
    {"register_concmd", register_concmd},
    {"register_cvar", register_cvar},
    {NULL, NULL}
};

// Plugin initialization
int AMXAPI amx_PluginInit(AMX *amx) {
    return amx_Register(amx, plugin_natives, -1);
}
```

#### 2. Event System Implementation
```cpp
// amxmodx_events.cpp - Event handling
struct event_t {
    const char *name;
    int id;
    int flags;
    void (*handler)(void);
};

// Event registration
int register_event(const char *event_name, const char *handler_name, int flags) {
    // Event registration logic
    return 1;
}

// Event dispatching
void dispatch_event(const char *event_name, int data[], int data_size) {
    // Event dispatch logic
}
```

#### 3. Native Function Implementation
```cpp
// amxmodx_natives.cpp - Native function implementation
static cell AMX_NATIVE_CALL get_user_health(AMX *amx, cell *params) {
    int id = params[1];
    if (id < 1 || id > gpGlobals->maxClients)
        return 0;
    
    edict_t *pEdict = INDEXENT(id);
    if (!pEdict || pEdict->free)
        return 0;
    
    return pEdict->v.health;
}
```

### ReHLDS Plugin Integration Deep Dive

#### 1. ReHLDS Native Functions
```cpp
// amxmodx_natives.cpp - ReHLDS specific natives
static cell AMX_NATIVE_CALL get_rehlds_version(AMX *amx, cell *params) {
    char *version;
    amx_StrParam(amx, params[1], version);
    strcpy(version, REHLDS_VERSION_STRING);
    return 1;
}

static cell AMX_NATIVE_CALL is_rehlds_server(AMX *amx, cell *params) {
    return 1; // Always true on ReHLDS
}

static cell AMX_NATIVE_CALL get_player_ping_avg(AMX *amx, cell *params) {
    int id = params[1];
    if (id < 1 || id > gpGlobals->maxClients)
        return 0;
    
    client_t *client = &g_psvs.clients[id - 1];
    return client->ping;
}
```

#### 2. Network Statistics Implementation
```cpp
// amxmodx_natives.cpp - Network monitoring
static cell AMX_NATIVE_CALL get_player_connection_quality(AMX *amx, cell *params) {
    int id = params[1];
    if (id < 1 || id > gpGlobals->maxClients)
        return CONNECTION_QUALITY_BAD;
    
    client_t *client = &g_psvs.clients[id - 1];
    int ping = client->ping;
    int loss = client->packet_loss;
    int choke = client->packet_choke;
    
    if (ping < 50 && loss < 1 && choke < 1)
        return CONNECTION_QUALITY_EXCELLENT;
    else if (ping < 100 && loss < 3 && choke < 3)
        return CONNECTION_QUALITY_GOOD;
    else if (ping < 150 && loss < 5 && choke < 5)
        return CONNECTION_QUALITY_FAIR;
    else if (ping < 200 && loss < 10 && choke < 10)
        return CONNECTION_QUALITY_POOR;
    else
        return CONNECTION_QUALITY_BAD;
}
```

#### 3. Security Implementation
```cpp
// amxmodx_natives.cpp - Security functions
static cell AMX_NATIVE_CALL is_player_rate_limited(AMX *amx, cell *params) {
    int id = params[1];
    if (id < 1 || id > gpGlobals->maxClients)
        return 0;
    
    client_t *client = &g_psvs.clients[id - 1];
    
    // Check move command rate
    if (client->movecmd_rate > sv_rehlds_movecmdrate_max_avg.value)
        return 1;
    
    // Check string command rate
    if (client->stringcmd_rate > sv_rehlds_stringcmdrate_max_avg.value)
        return 1;
    
    return 0;
}

static cell AMX_NATIVE_CALL check_file_integrity(AMX *amx, cell *params) {
    char *filename;
    amx_StrParam(amx, params[1], filename);
    
    // Calculate file hash
    char hash[33];
    calculate_file_hash(filename, hash);
    
    // Compare with known good hash
    if (strcmp(hash, get_known_hash(filename)) == 0)
        return INTEGRITY_CHECK_PASS;
    else
        return INTEGRITY_CHECK_FAIL;
}
```

## Plugin Development Best Practices

### 1. Memory Management
```pawn
// Proper memory allocation
new g_player_data[33][MAX_PLAYER_DATA]

// Cleanup on disconnect
public client_disconnect(id) {
    // Clear player data
    for (new i = 0; i < MAX_PLAYER_DATA; i++) {
        g_player_data[id][i] = 0
    }
}
```

### 2. Error Handling
```pawn
// Always check return values
public plugin_init() {
    new result = register_plugin("My Plugin", "1.0", "Author")
    if (result != 1) {
        log_amx("Failed to register plugin")
        return
    }
    
    // Check if required modules are loaded
    if (!module_exists("fun")) {
        log_amx("Fun module not found")
        return
    }
}
```

### 3. Performance Optimization
```pawn
// Use efficient loops
public check_all_players() {
    new players[32], num
    get_players(players, num)
    
    for (new i = 0; i < num; i++) {
        new id = players[i]
        // Process player
    }
}

// Cache frequently used values
new g_max_players
public plugin_init() {
    g_max_players = get_maxplayers()
}
```

### 4. Security Implementation
```pawn
// Input validation
public cmd_admin_action(id, level, cid) {
    if (!cmd_access(id, level, cid, 1))
        return PLUGIN_HANDLED
    
    new target[32]
    read_argv(1, target, charsmax(target))
    
    // Validate target
    new target_id = find_player("a", target)
    if (target_id == 0) {
        client_print(id, print_console, "Player not found")
        return PLUGIN_HANDLED
    }
    
    // Perform action
    return PLUGIN_HANDLED
}
```

## Source Code Navigation

### Key Files for Plugin Development

#### AMX Mod X
- `amxmodx/amxmodx.cpp` - Main plugin loader
- `amxmodx/amxmodx_impl.cpp` - Core implementation
- `modules/fun/fun.cpp` - Fun module implementation
- `modules/engine/engine.cpp` - Engine module implementation
- `public/amxmodx.h` - Main header file

#### ReHLDS
- `rehlds/engine/sv_main.cpp` - Main server logic
- `rehlds/engine/sv_user.cpp` - User management
- `rehlds/network/net_main.cpp` - Network handling
- `rehlds/dlls/amxmodx.cpp` - AMX Mod X integration
- `rehlds/common/common.cpp` - Common utilities

### Debugging and Development

#### Compilation Process
```bash
# AMX Mod X plugin compilation
amxxpc plugin.sma -o plugin.amxx

# ReHLDS compilation
./build.sh --compiler=gcc --jobs=4
```

#### Debug Information
```pawn
// Enable debug mode
#define DEBUG_MODE

#if defined DEBUG_MODE
    #define DEBUG_LOG(%1) log_amx(%1)
#else
    #define DEBUG_LOG(%1)
#endif

public plugin_init() {
    DEBUG_LOG("Plugin initialized")
}
```

This comprehensive source reference guide provides all the necessary details for AI to understand the complete plugin development ecosystem for both AMX Mod X and ReHLDS.
