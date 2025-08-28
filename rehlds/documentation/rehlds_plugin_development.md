# ReHLDS - Plugin Development Guide

## Overview
This guide covers plugin development for ReHLDS using Pawn programming language with AMX Mod X. ReHLDS provides additional native functions and include files specifically designed for enhanced server functionality.

## Include Files

### Core ReHLDS Include
```pawn
#include <rehlds>
```

### Additional Includes
```pawn
#include <amxmodx>
#include <fakemeta>
#include <fun>
#include <engine>
#include <sqlx>
#include <rehlds_const>
#include <rehlds_funcs>
```

## ReHLDS Native Functions

### Server Information Functions
```pawn
// Get ReHLDS version information
native get_rehlds_version()
native get_rehlds_build()
native get_rehlds_revision()

// Server status functions
native is_rehlds_server()
native get_server_uptime()
native get_server_fps()
native get_server_tickrate()
```

### Enhanced Player Functions
```pawn
// ReHLDS specific player functions
native get_player_ping_avg(id)
native get_player_ping_burst(id)
native get_player_loss_avg(id)
native get_player_loss_burst(id)
native get_player_choke_avg(id)
native get_player_choke_burst(id)

// Player connection quality
native get_player_connection_quality(id)
native is_player_lagging(id)
native get_player_lag_level(id)

// Enhanced player data
native get_player_steam_id(id, steamid[], len)
native get_player_ip_address(id, ip[], len, with_port = 0)
native get_player_connection_time(id)
native get_player_last_activity(id)
```

### Network and Protocol Functions
```pawn
// Network monitoring
native get_server_network_stats()
native get_player_network_stats(id)
native get_server_bandwidth_usage()
native get_player_bandwidth_usage(id)

// Protocol functions
native get_protocol_version()
native is_protocol_compatible(version)
native get_client_protocol(id)
```

### Security and Anti-Cheat Functions
```pawn
// File integrity checking
native check_file_integrity(const filename[])
native is_file_modified(const filename[])
native get_file_hash(const filename[], hash[], len)

// Rate limiting functions
native get_player_command_rate(id)
native get_player_move_rate(id)
native get_player_string_rate(id)
native is_player_rate_limited(id)

// Decompression protection
native get_decompression_failures(id)
native is_decompression_suspicious(id)
native reset_decompression_failures(id)
```

### Performance Monitoring Functions
```pawn
// Server performance
native get_server_cpu_usage()
native get_server_memory_usage()
native get_server_network_usage()
native get_server_entity_count()
native get_server_client_count()

// Resource management
native get_precached_resources_count()
native get_precached_sounds_count()
native get_precached_models_count()
native get_precached_decal_count()
native get_precached_generic_count()
native get_precached_event_count()
```

### Advanced Server Functions
```pawn
// Entity file management
native use_custom_entity_file(const mapname[])
native create_entity_file(const mapname[])
native get_entity_file_path(const mapname[], path[], len)

// Random seed management
native set_custom_random_seed(seed)
native get_custom_random_seed()
native generate_custom_random_seed()

// Userinfo field filtering
native set_userinfo_filter(const fields[])
native get_userinfo_filter(fields[], len)
native is_userinfo_field_allowed(const field[])
```

## ReHLDS Constants

### Version Constants
```pawn
#define REHLDS_VERSION_MAJOR    3
#define REHLDS_VERSION_MINOR    14
#define REHLDS_VERSION_PATCH    0
#define REHLDS_VERSION_BUILD    857

#define REHLDS_VERSION_STRING   "3.14.0.857"
```

### Connection Quality Constants
```pawn
#define CONNECTION_QUALITY_EXCELLENT  0
#define CONNECTION_QUALITY_GOOD       1
#define CONNECTION_QUALITY_FAIR       2
#define CONNECTION_QUALITY_POOR       3
#define CONNECTION_QUALITY_BAD        4

#define LAG_LEVEL_NONE                0
#define LAG_LEVEL_LOW                 1
#define LAG_LEVEL_MEDIUM              2
#define LAG_LEVEL_HIGH                3
#define LAG_LEVEL_EXTREME             4
```

### Rate Limiting Constants
```pawn
#define RATE_LIMIT_NONE               0
#define RATE_LIMIT_WARNING            1
#define RATE_LIMIT_KICK               2
#define RATE_LIMIT_BAN                3

#define COMMAND_TYPE_MOVE             0
#define COMMAND_TYPE_STRING           1
#define COMMAND_TYPE_PACKET           2
```

### Security Constants
```pawn
#define SECURITY_LEVEL_LOW            0
#define SECURITY_LEVEL_MEDIUM         1
#define SECURITY_LEVEL_HIGH           2
#define SECURITY_LEVEL_EXTREME        3

#define INTEGRITY_CHECK_PASS          0
#define INTEGRITY_CHECK_FAIL          1
#define INTEGRITY_CHECK_UNKNOWN       2
```

## Plugin Development Examples

### Basic ReHLDS Plugin Structure
```pawn
#include <amxmodx>
#include <rehlds>

#define PLUGIN_NAME "ReHLDS Enhanced Plugin"
#define PLUGIN_VERSION "1.0"
#define PLUGIN_AUTHOR "Developer"

public plugin_init() {
    register_plugin(PLUGIN_NAME, PLUGIN_VERSION, PLUGIN_AUTHOR)
    
    // Check if running on ReHLDS
    if (!is_rehlds_server()) {
        log_amx("Warning: This plugin requires ReHLDS server")
        return
    }
    
    // Display ReHLDS version
    new version[32]
    get_rehlds_version(version, charsmax(version))
    log_amx("ReHLDS Version: %s", version)
    
    return PLUGIN_CONTINUE
}
```

### Player Connection Quality Monitor
```pawn
#include <amxmodx>
#include <rehlds>

new g_connection_warnings[33]

public client_putinserver(id) {
    if (!is_user_connected(id))
        return PLUGIN_CONTINUE
    
    // Reset connection warnings
    g_connection_warnings[id] = 0
    
    // Start monitoring connection quality
    set_task(10.0, "check_connection_quality", id, _, _, "b")
    
    return PLUGIN_CONTINUE
}

public check_connection_quality(id) {
    if (!is_user_connected(id))
        return
    
    new quality = get_player_connection_quality(id)
    new lag_level = get_player_lag_level(id)
    
    // Check for poor connection
    if (quality >= CONNECTION_QUALITY_POOR || lag_level >= LAG_LEVEL_HIGH) {
        g_connection_warnings[id]++
        
        new name[32]
        get_user_name(id, name, charsmax(name))
        
        if (g_connection_warnings[id] >= 3) {
            client_print(id, print_chat, "[ReHLDS] Your connection quality is poor. Consider improving your internet connection.")
            log_amx("Player %s has poor connection quality (Warnings: %d)", name, g_connection_warnings[id])
        }
    }
}
```

### Network Statistics Plugin
```pawn
#include <amxmodx>
#include <rehlds>

public plugin_init() {
    register_plugin("Network Statistics", "1.0", "Developer")
    register_clcmd("say /netstats", "cmd_netstats")
    register_clcmd("say /serverstats", "cmd_serverstats")
    
    return PLUGIN_CONTINUE
}

public cmd_netstats(id) {
    if (!is_user_connected(id))
        return PLUGIN_HANDLED
    
    new name[32]
    get_user_name(id, name, charsmax(name))
    
    new ping_avg = get_player_ping_avg(id)
    new ping_burst = get_player_ping_burst(id)
    new loss_avg = get_player_loss_avg(id)
    new loss_burst = get_player_loss_burst(id)
    new choke_avg = get_player_choke_avg(id)
    new choke_burst = get_player_choke_burst(id)
    
    client_print(id, print_chat, "[NetStats] %s:", name)
    client_print(id, print_chat, "Ping: %d avg, %d burst", ping_avg, ping_burst)
    client_print(id, print_chat, "Loss: %d avg, %d burst", loss_avg, loss_burst)
    client_print(id, print_chat, "Choke: %d avg, %d burst", choke_avg, choke_burst)
    
    return PLUGIN_HANDLED
}

public cmd_serverstats(id) {
    if (!is_user_connected(id))
        return PLUGIN_HANDLED
    
    new cpu_usage = get_server_cpu_usage()
    new memory_usage = get_server_memory_usage()
    new network_usage = get_server_network_usage()
    new entity_count = get_server_entity_count()
    new client_count = get_server_client_count()
    new uptime = get_server_uptime()
    
    client_print(id, print_chat, "[ServerStats] Server Performance:")
    client_print(id, print_chat, "CPU: %d%%, Memory: %d%%, Network: %d%%", cpu_usage, memory_usage, network_usage)
    client_print(id, print_chat, "Entities: %d, Clients: %d, Uptime: %d seconds", entity_count, client_count, uptime)
    
    return PLUGIN_HANDLED
}
```

### Security Monitoring Plugin
```pawn
#include <amxmodx>
#include <rehlds>

new g_security_alerts[33]

public plugin_init() {
    register_plugin("Security Monitor", "1.0", "Developer")
    
    // Monitor for suspicious activity
    set_task(5.0, "check_security", _, _, _, "b")
    
    return PLUGIN_CONTINUE
}

public check_security() {
    new players[32], num, id
    get_players(players, num)
    
    for (new i = 0; i < num; i++) {
        id = players[i]
        
        if (!is_user_connected(id))
            continue
        
        // Check for rate limiting
        if (is_player_rate_limited(id)) {
            g_security_alerts[id]++
            
            new name[32]
            get_user_name(id, name, charsmax(name))
            
            log_amx("Security Alert: Player %s is rate limited (Alerts: %d)", name, g_security_alerts[id])
            
            if (g_security_alerts[id] >= 5) {
                server_cmd("kick #%d Rate limiting violation", get_user_userid(id))
            }
        }
        
        // Check for decompression issues
        if (is_decompression_suspicious(id)) {
            new name[32]
            get_user_name(id, name, charsmax(name))
            
            log_amx("Security Alert: Player %s has suspicious decompression activity", name)
        }
    }
}
```

### File Integrity Checker
```pawn
#include <amxmodx>
#include <rehlds>

public plugin_init() {
    register_plugin("File Integrity Checker", "1.0", "Developer")
    register_clcmd("say /checkfiles", "cmd_check_files")
    
    return PLUGIN_CONTINUE
}

public cmd_check_files(id) {
    if (!is_user_admin(id))
        return PLUGIN_HANDLED
    
    new files[][] = {
        "cstrike/maps/de_dust2.bsp",
        "cstrike/maps/de_inferno.bsp",
        "cstrike/maps/de_nuke.bsp",
        "cstrike/maps/de_train.bsp"
    }
    
    client_print(id, print_chat, "[FileCheck] Checking file integrity...")
    
    for (new i = 0; i < sizeof(files); i++) {
        new integrity = check_file_integrity(files[i])
        
        switch (integrity) {
            case INTEGRITY_CHECK_PASS: {
                client_print(id, print_chat, "[FileCheck] %s: OK", files[i])
            }
            case INTEGRITY_CHECK_FAIL: {
                client_print(id, print_chat, "[FileCheck] %s: MODIFIED", files[i])
                log_amx("File integrity check failed: %s", files[i])
            }
            case INTEGRITY_CHECK_UNKNOWN: {
                client_print(id, print_chat, "[FileCheck] %s: UNKNOWN", files[i])
            }
        }
    }
    
    return PLUGIN_HANDLED
}
```

### Performance Monitor Plugin
```pawn
#include <amxmodx>
#include <rehlds>

new g_performance_warnings = 0

public plugin_init() {
    register_plugin("Performance Monitor", "1.0", "Developer")
    
    // Monitor server performance
    set_task(30.0, "check_performance", _, _, _, "b")
    
    return PLUGIN_CONTINUE
}

public check_performance() {
    new cpu_usage = get_server_cpu_usage()
    new memory_usage = get_server_memory_usage()
    new fps = get_server_fps()
    
    // Check for performance issues
    if (cpu_usage > 80 || memory_usage > 80 || fps < 60) {
        g_performance_warnings++
        
        log_amx("Performance Warning: CPU=%d%%, Memory=%d%%, FPS=%d", cpu_usage, memory_usage, fps)
        
        if (g_performance_warnings >= 3) {
            log_amx("CRITICAL: Server performance is degraded")
            g_performance_warnings = 0
        }
    } else {
        g_performance_warnings = 0
    }
    
    // Log resource usage
    new sound_count = get_precached_sounds_count()
    new model_count = get_precached_models_count()
    new decal_count = get_precached_decal_count()
    
    log_amx("Resource Usage: Sounds=%d, Models=%d, Decals=%d", sound_count, model_count, decal_count)
}
```

## Best Practices

### 1. ReHLDS Compatibility
```pawn
// Always check if running on ReHLDS
if (!is_rehlds_server()) {
    log_amx("Warning: Plugin requires ReHLDS server")
    return PLUGIN_CONTINUE
}
```

### 2. Performance Monitoring
```pawn
// Monitor server performance regularly
// Use appropriate task intervals
// Log performance issues
```

### 3. Security Implementation
```pawn
// Implement rate limiting checks
// Monitor for suspicious activity
// Use file integrity checking
```

### 4. Error Handling
```pawn
// Always check return values
// Handle connection quality issues
// Implement proper logging
```

## Integration with AMX Mod X

### Combining ReHLDS and AMX Mod X Functions
```pawn
#include <amxmodx>
#include <rehlds>
#include <fakemeta>
#include <fun>

public plugin_init() {
    register_plugin("ReHLDS + AMX Mod X Plugin", "1.0", "Developer")
    
    // Use both ReHLDS and AMX Mod X functions
    register_clcmd("say /fullstats", "cmd_full_stats")
    
    return PLUGIN_CONTINUE
}

public cmd_full_stats(id) {
    if (!is_user_connected(id))
        return PLUGIN_HANDLED
    
    // AMX Mod X functions
    new health = get_user_health(id)
    new armor = get_user_armor(id)
    new frags = get_user_frags(id)
    new deaths = get_user_deaths(id)
    
    // ReHLDS functions
    new ping_avg = get_player_ping_avg(id)
    new loss_avg = get_player_loss_avg(id)
    new quality = get_player_connection_quality(id)
    
    client_print(id, print_chat, "[FullStats] Health: %d, Armor: %d", health, armor)
    client_print(id, print_chat, "[FullStats] Frags: %d, Deaths: %d", frags, deaths)
    client_print(id, print_chat, "[FullStats] Ping: %d, Loss: %d, Quality: %d", ping_avg, loss_avg, quality)
    
    return PLUGIN_HANDLED
}
```

This comprehensive guide provides all the necessary information for developing plugins specifically for ReHLDS using Pawn programming language with AMX Mod X integration.
