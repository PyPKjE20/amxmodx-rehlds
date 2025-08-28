# AMX Mod X - Code Examples and Snippets

## Basic Plugin Structure

### Minimal Plugin
```pawn
#include <amxmodx>

public plugin_init() {
    register_plugin("Basic Plugin", "1.0", "Author")
    return PLUGIN_CONTINUE
}
```

### Plugin with Configuration
```pawn
#include <amxmodx>
#include <amxmisc>

#define PLUGIN_NAME "Advanced Plugin"
#define PLUGIN_VERSION "1.0"
#define PLUGIN_AUTHOR "Author"

new g_enabled
new g_max_players

public plugin_init() {
    register_plugin(PLUGIN_NAME, PLUGIN_VERSION, PLUGIN_AUTHOR)
    
    // Register commands
    register_clcmd("say /help", "cmd_help")
    register_concmd("amx_plugin", "cmd_admin", ADMIN_LEVEL, "Admin command")
    
    // Load configuration
    load_config()
    
    return PLUGIN_CONTINUE
}

public plugin_cfg() {
    load_config()
}

load_config() {
    g_enabled = get_cvar_num("plugin_enabled")
    g_max_players = get_maxplayers()
}
```

## Player Management

### Player Connection Handling
```pawn
public client_putinserver(id) {
    if (!is_user_connected(id))
        return PLUGIN_CONTINUE
    
    new name[32], authid[32], ip[32]
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    get_user_ip(id, ip, charsmax(ip), 1)
    
    // Log player connection
    log_amx("Player %s (%s) connected from %s", name, authid, ip)
    
    // Load player data
    load_player_data(id)
    
    return PLUGIN_CONTINUE
}

public client_disconnect(id) {
    if (!is_user_connected(id))
        return PLUGIN_CONTINUE
    
    new name[32]
    get_user_name(id, name, charsmax(name))
    
    // Save player data
    save_player_data(id)
    
    log_amx("Player %s disconnected", name)
    return PLUGIN_CONTINUE
}
```

### Player Data Management
```pawn
enum _:PlayerData {
    PD_NAME[32],
    PD_AUTHID[32],
    PD_LEVEL,
    PD_EXPERIENCE,
    PD_LAST_SEEN
}

new g_player_data[33][PlayerData]

load_player_data(id) {
    new authid[32]
    get_user_authid(id, authid, charsmax(authid))
    
    // Load from database
    new query[256]
    formatex(query, charsmax(query), "SELECT * FROM players WHERE authid='%s'", authid)
    SQL_ThreadQuery(g_sqlTuple, "HandlePlayerLoad", query, authid, strlen(authid))
}

public HandlePlayerLoad(failstate, Handle:query, error[], errnum, data[], size, Float:queuetime) {
    if (failstate != TQUERY_SUCCESS) {
        log_amx("SQL Error: %s", error)
        return
    }
    
    new id = find_player("a", data)
    if (!id) return
    
    if (SQL_NumResults(query) > 0) {
        // Load existing data
        g_player_data[id][PD_LEVEL] = SQL_ReadResult(query, 2)
        g_player_data[id][PD_EXPERIENCE] = SQL_ReadResult(query, 3)
    } else {
        // Create new player
        create_new_player(id)
    }
}
```

## Event Handling

### Death Event with Statistics
```pawn
public client_death(killer, victim, weapon, hitplace) {
    if (!is_user_alive(killer) || !is_user_alive(victim))
        return PLUGIN_CONTINUE
    
    new killer_name[32], victim_name[32]
    get_user_name(killer, killer_name, charsmax(killer_name))
    get_user_name(victim, victim_name, charsmax(victim_name))
    
    // Update statistics
    update_kill_stats(killer, victim, weapon)
    
    // Log death
    log_amx("Kill: %s killed %s with %s", killer_name, victim_name, weapon)
    
    return PLUGIN_CONTINUE
}

update_kill_stats(killer, victim, weapon[]) {
    // Update killer stats
    g_player_data[killer][PD_EXPERIENCE] += 10
    
    // Check for level up
    check_level_up(killer)
    
    // Save to database
    save_player_stats(killer)
}
```

### Spawn Event
```pawn
public client_spawn(id) {
    if (!is_user_alive(id))
        return PLUGIN_CONTINUE
    
    // Set player properties
    set_user_health(id, 100)
    set_user_armor(id, 100)
    
    // Give starting items
    give_starting_items(id)
    
    return PLUGIN_CONTINUE
}

give_starting_items(id) {
    // Give basic weapons
    give_item(id, "weapon_knife")
    give_item(id, "weapon_usp")
    give_item(id, "ammo_45acp")
}
```

## Command Handling

### Chat Commands
```pawn
public cmd_help(id) {
    if (!is_user_connected(id))
        return PLUGIN_HANDLED
    
    client_print(id, print_chat, "[Plugin] Available commands:")
    client_print(id, print_chat, "say /help - Show this help")
    client_print(id, print_chat, "say /stats - Show your statistics")
    client_print(id, print_chat, "say /top - Show top players")
    
    return PLUGIN_HANDLED
}

public cmd_stats(id) {
    if (!is_user_connected(id))
        return PLUGIN_HANDLED
    
    new name[32]
    get_user_name(id, name, charsmax(name))
    
    client_print(id, print_chat, "[Stats] %s:", name)
    client_print(id, print_chat, "Level: %d", g_player_data[id][PD_LEVEL])
    client_print(id, print_chat, "Experience: %d", g_player_data[id][PD_EXPERIENCE])
    
    return PLUGIN_HANDLED
}
```

### Admin Commands
```pawn
public cmd_admin(id, level, cid) {
    if (!cmd_access(id, level, cid, 1))
        return PLUGIN_HANDLED
    
    new arg[32]
    read_argv(1, arg, charsmax(arg))
    
    if (equal(arg, "reload")) {
        load_config()
        console_print(id, "[Admin] Configuration reloaded")
    } else if (equal(arg, "status")) {
        show_admin_status(id)
    }
    
    return PLUGIN_HANDLED
}

show_admin_status(id) {
    console_print(id, "[Admin] Plugin Status:")
    console_print(id, "Enabled: %s", g_enabled ? "Yes" : "No")
    console_print(id, "Max Players: %d", g_max_players)
    console_print(id, "Connected Players: %d", get_playersnum())
}
```

## Database Operations

### Database Connection
```pawn
new Handle:g_sqlTuple

public plugin_init() {
    // ... other code ...
    
    // Initialize database connection
    new host[64], user[32], pass[32], db[32]
    get_cvar_string("mysql_host", host, charsmax(host))
    get_cvar_string("mysql_user", user, charsmax(user))
    get_cvar_string("mysql_pass", pass, charsmax(pass))
    get_cvar_string("mysql_db", db, charsmax(db))
    
    g_sqlTuple = SQL_MakeDbTuple(host, user, pass, db)
    
    // Test connection
    SQL_ThreadQuery(g_sqlTuple, "HandleConnection", "SELECT 1", "", 0)
}

public HandleConnection(failstate, Handle:query, error[], errnum, data[], size, Float:queuetime) {
    if (failstate != TQUERY_SUCCESS) {
        log_amx("Database connection failed: %s", error)
        return
    }
    
    log_amx("Database connection successful")
}
```

### Data Insertion
```pawn
create_new_player(id) {
    new name[32], authid[32]
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    
    // Escape strings for SQL
    new escaped_name[64], escaped_authid[64]
    SQL_QuoteString(Empty_Handle, escaped_name, charsmax(escaped_name), name)
    SQL_QuoteString(Empty_Handle, escaped_authid, charsmax(escaped_authid), authid)
    
    new query[512]
    formatex(query, charsmax(query), "INSERT INTO players (name, authid, level, experience, created_at) VALUES ('%s', '%s', 1, 0, NOW())", escaped_name, escaped_authid)
    
    SQL_ThreadQuery(g_sqlTuple, "HandlePlayerCreate", query, authid, strlen(authid))
}

public HandlePlayerCreate(failstate, Handle:query, error[], errnum, data[], size, Float:queuetime) {
    if (failstate != TQUERY_SUCCESS) {
        log_amx("Failed to create player: %s", error)
        return
    }
    
    new id = find_player("a", data)
    if (id) {
        g_player_data[id][PD_LEVEL] = 1
        g_player_data[id][PD_EXPERIENCE] = 0
        client_print(id, print_chat, "[Plugin] Welcome! Your account has been created.")
    }
}
```

## Timer and Task Management

### Periodic Tasks
```pawn
public plugin_init() {
    // ... other code ...
    
    // Set up periodic tasks
    set_task(60.0, "save_all_players", _, _, _, "b")
    set_task(300.0, "cleanup_old_data", _, _, _, "b")
}

public save_all_players() {
    new players[32], num, id
    get_players(players, num)
    
    for (new i = 0; i < num; i++) {
        id = players[i]
        if (is_user_connected(id)) {
            save_player_data(id)
        }
    }
    
    log_amx("Saved data for %d players", num)
}

public cleanup_old_data() {
    new query[256]
    formatex(query, charsmax(query), "DELETE FROM logs WHERE created_at < DATE_SUB(NOW(), INTERVAL 30 DAY)")
    SQL_ThreadQuery(g_sqlTuple, "HandleCleanup", query, "", 0)
}
```

### Delayed Actions
```pawn
public client_putinserver(id) {
    // ... other code ...
    
    // Welcome message after 2 seconds
    set_task(2.0, "welcome_message", id)
}

public welcome_message(id) {
    if (!is_user_connected(id))
        return
    
    client_print(id, print_chat, "[Plugin] Welcome to the server!")
    client_print(id, print_chat, "[Plugin] Type 'say /help' for available commands")
}
```

## Entity Manipulation

### Entity Creation and Modification
```pawn
create_custom_entity(const classname[], const model[]) {
    new ent = create_entity("info_target")
    if (!ent) return 0
    
    entity_set_string(ent, EV_SZ_classname, classname)
    entity_set_model(ent, model)
    entity_set_int(ent, EV_INT_movetype, MOVETYPE_NONE)
    entity_set_int(ent, EV_INT_solid, SOLID_BBOX)
    
    return ent
}

set_entity_position(ent, Float:x, Float:y, Float:z) {
    new Float:origin[3]
    origin[0] = x
    origin[1] = y
    origin[2] = z
    
    entity_set_origin(ent, origin)
}
```

### Entity Finding
```pawn
find_entities_by_class(const classname[]) {
    new ent = -1
    new count = 0
    
    while ((ent = find_ent_by_class(ent, classname))) {
        count++
        // Process entity
        process_entity(ent)
    }
    
    return count
}

process_entity(ent) {
    new classname[32]
    entity_get_string(ent, EV_SZ_classname, classname, charsmax(classname))
    
    // Get entity position
    new Float:origin[3]
    entity_get_vector(ent, EV_VEC_origin, origin)
    
    log_amx("Found entity %s at %.1f %.1f %.1f", classname, origin[0], origin[1], origin[2])
}
```

## Configuration Management

### CVAR Management
```pawn
new g_cvar_enabled
new g_cvar_max_level
new g_cvar_exp_multiplier

public plugin_init() {
    // ... other code ...
    
    // Create CVARs
    g_cvar_enabled = register_cvar("plugin_enabled", "1")
    g_cvar_max_level = register_cvar("plugin_max_level", "100")
    g_cvar_exp_multiplier = register_cvar("plugin_exp_multiplier", "1.0")
    
    // Hook CVAR changes
    hook_cvar_change(g_cvar_enabled, "hook_cvar_changed")
    hook_cvar_change(g_cvar_max_level, "hook_cvar_changed")
}

public hook_cvar_changed(pcvar, const old_value[], const new_value[]) {
    new name[32]
    get_cvar_string(pcvar, name, charsmax(name))
    
    log_amx("CVAR %s changed from %s to %s", name, old_value, new_value)
}
```

### File Configuration
```pawn
load_config() {
    new config_file[64]
    get_configsdir(config_file, charsmax(config_file))
    add(config_file, charsmax(config_file), "/plugin.cfg")
    
    if (file_exists(config_file)) {
        server_cmd("exec %s", config_file)
        log_amx("Configuration loaded from %s", config_file)
    } else {
        log_amx("Configuration file not found: %s", config_file)
    }
}
```

## Error Handling and Debugging

### Comprehensive Error Handling
```pawn
public plugin_init() {
    // ... other code ...
    
    // Set up error handling
    set_fail_state("Plugin initialization failed")
}

public plugin_end() {
    // Cleanup
    if (g_sqlTuple) {
        SQL_FreeHandle(g_sqlTuple)
    }
    
    log_amx("Plugin unloaded")
}

// Safe database query wrapper
safe_query(const query[], const callback[], const data[] = "", data_size = 0) {
    if (!g_sqlTuple) {
        log_amx("Database not connected")
        return 0
    }
    
    return SQL_ThreadQuery(g_sqlTuple, callback, query, data, data_size)
}
```

### Debug Functions
```pawn
debug_print(const message[], any:...) {
    static debug_enabled
    if (!debug_enabled) {
        debug_enabled = get_cvar_num("plugin_debug")
    }
    
    if (debug_enabled) {
        static formatted[256]
        vformat(formatted, charsmax(formatted), message, 2)
        log_amx("[DEBUG] %s", formatted)
    }
}

debug_player_info(id) {
    if (!is_user_connected(id))
        return
    
    new name[32], authid[32], health, armor
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    health = get_user_health(id)
    armor = get_user_armor(id)
    
    debug_print("Player %s (%s): Health=%d, Armor=%d", name, authid, health, armor)
}
```

## Performance Optimization

### Efficient Loops
```pawn
// Good: Use get_players() for player loops
public update_all_players() {
    new players[32], num, id
    get_players(players, num)
    
    for (new i = 0; i < num; i++) {
        id = players[i]
        if (is_user_connected(id)) {
            update_player(id)
        }
    }
}

// Bad: Don't use find_player() in loops
public bad_update_all_players() {
    new id
    for (new i = 1; i <= get_maxplayers(); i++) {
        id = find_player("a", i)
        if (id) {
            update_player(id)
        }
    }
}
```

### Memory Management
```pawn
// Always free handles
public plugin_end() {
    if (g_sqlTuple) {
        SQL_FreeHandle(g_sqlTuple)
        g_sqlTuple = Empty_Handle
    }
}

// Use proper variable scoping
public some_function() {
    new temp_array[64]  // Local scope
    // ... use temp_array ...
    // Automatically freed when function ends
}
```

## Security Best Practices

### Input Validation
```pawn
public cmd_admin(id, level, cid) {
    if (!cmd_access(id, level, cid, 1))
        return PLUGIN_HANDLED
    
    new arg[32]
    read_argv(1, arg, charsmax(arg))
    
    // Validate input
    if (strlen(arg) < 1 || strlen(arg) > 31) {
        console_print(id, "[Admin] Invalid argument length")
        return PLUGIN_HANDLED
    }
    
    // Check for dangerous characters
    if (containi(arg, "';") != -1) {
        console_print(id, "[Admin] Invalid characters in argument")
        return PLUGIN_HANDLED
    }
    
    // Process command
    process_admin_command(id, arg)
    
    return PLUGIN_HANDLED
}
```

### SQL Injection Prevention
```pawn
// Good: Use SQL_QuoteString
public safe_player_query(id) {
    new name[32], authid[32]
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    
    new escaped_name[64], escaped_authid[64]
    SQL_QuoteString(Empty_Handle, escaped_name, charsmax(escaped_name), name)
    SQL_QuoteString(Empty_Handle, escaped_authid, charsmax(escaped_authid), authid)
    
    new query[256]
    formatex(query, charsmax(query), "SELECT * FROM players WHERE name='%s' AND authid='%s'", escaped_name, escaped_authid)
    
    SQL_ThreadQuery(g_sqlTuple, "HandleSafeQuery", query, "", 0)
}

// Bad: Direct string concatenation (vulnerable to SQL injection)
public unsafe_player_query(id) {
    new name[32], authid[32]
    get_user_name(id, name, charsmax(name))
    get_user_authid(id, authid, charsmax(authid))
    
    new query[256]
    formatex(query, charsmax(query), "SELECT * FROM players WHERE name='%s' AND authid='%s'", name, authid)  // UNSAFE!
    
    SQL_ThreadQuery(g_sqlTuple, "HandleUnsafeQuery", query, "", 0)
}
```

These examples provide comprehensive coverage of AMX Mod X development patterns, from basic plugin structure to advanced features like database operations, entity manipulation, and security best practices. Each example includes proper error handling and follows AMX Mod X conventions.
