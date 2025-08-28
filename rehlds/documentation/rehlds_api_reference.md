# ReHLDS - API Reference and CVAR Documentation

## Core CVARs

### Server Configuration
```bash
# Basic Server Settings
hostname "Server Name"                    # Server display name
sv_password ""                           # Server password (empty = public)
rcon_password "password"                 # RCON access password
sv_region 3                              # Server region (0-255)
sv_lan 0                                 # LAN mode (0=Internet, 1=LAN)
sv_maxplayers 32                         # Maximum players
sv_gravity 800                           # World gravity
sv_friction 4                            # Surface friction
sv_airaccelerate 10                      # Air acceleration
sv_wateraccelerate 10                    # Water acceleration
sv_accelerate 5                          # Ground acceleration
sv_stopspeed 75                          # Stop speed
sv_stepsize 18                           # Step size
sv_waterdist 12                          # Water distance
sv_waterfriction 1                       # Water friction
sv_waterjump 0                           # Water jumping
sv_maxspeed 320                          # Maximum player speed
```

### Network Configuration
```bash
# Network Settings
sv_maxrate 25000                         # Maximum client rate
sv_minrate 0                             # Minimum client rate
sv_maxupdaterate 101                     # Maximum update rate
sv_minupdaterate 30                      # Minimum update rate
sv_timeout 65                            # Server timeout
cl_timeout 65                            # Client timeout
sv_maxpacket_loss 0                      # Maximum packet loss
sv_maxpacket_loss_avg 0                  # Average packet loss
sv_maxpacket_loss_burst 0                # Burst packet loss
sv_maxpacket_loss_avg_punish 0           # Average packet loss punishment
sv_maxpacket_loss_burst_punish 0         # Burst packet loss punishment
```

### Game Settings
```bash
# Counter-Strike Specific
mp_timelimit 30                          # Time limit per map
mp_roundtime 5                           # Round time limit
mp_freezetime 6                          # Freeze time
mp_buytime 1.5                           # Buy time
mp_c4timer 35                            # C4 timer
mp_fraglimit 0                           # Frag limit
mp_maxrounds 0                           # Maximum rounds
mp_winlimit 0                            # Win limit
mp_autoteambalance 1                     # Auto team balance
mp_autokick 0                            # Auto kick
mp_flashlight 1                          # Flashlight enabled
mp_footsteps 1                           # Footsteps enabled
mp_limitteams 2                          # Team limit
mp_teamlist "hgrunt;scientist"           # Team list
mp_allowspectators 1                     # Allow spectators
mp_chattime 10                           # Chat time
mp_forcecamera 0                         # Force camera
mp_friendlyfire 0                        # Friendly fire
mp_friendly_grenade_damage 0             # Friendly grenade damage
mp_tkpunish 0                            # Team kill punishment
```

## ReHLDS Specific CVARs

### Security and Anti-Cheat
```bash
# File Integrity
sv_rehlds_force_unmodified 1             # Force unmodified files

# Packet Filtering
sv_filterban 1                           # Enable packet filtering
sv_filterban_mode 1                      # Filtering mode (-1=reject all, 0=no check, 1=check bans)

# Rate Limiting
sv_rehlds_movecmdrate_max_avg 400        # Max average move command rate
sv_rehlds_movecmdrate_avg_punish 5       # Move command rate punishment (minutes)
sv_rehlds_movecmdrate_max_burst 2500     # Max burst move command rate
sv_rehlds_movecmdrate_burst_punish 5     # Burst move command punishment (minutes)
sv_rehlds_stringcmdrate_max_avg 80       # Max average string command rate
sv_rehlds_stringcmdrate_avg_punish 5     # String command rate punishment (minutes)
sv_rehlds_stringcmdrate_max_burst 400    # Max burst string command rate
sv_rehlds_stringcmdrate_burst_punish 5   # Burst string command punishment (minutes)

# Connection Limits
sv_rehlds_maxclients_from_single_ip 5    # Max clients from single IP
```

### Decompression Protection
```bash
# Bzip2 Decompression
sv_net_incoming_decompression 1          # Enable decompression
sv_net_incoming_decompression_max_ratio 80.0  # Max compression ratio
sv_net_incoming_decompression_max_size 65536  # Max decompressed size
sv_net_incoming_decompression_min_failures 4  # Min failures before action
sv_net_incoming_decompression_max_failures 10 # Max failures allowed
sv_net_incoming_decompression_min_failuretime 0.1  # Min failure time window
sv_net_incoming_decompression_punish -1  # Punishment for decompression failures
```

### Performance Optimization
```bash
# Network Optimization
sv_rehlds_maxpacket_loss 0               # Max packet loss
sv_rehlds_hull_centering 0               # Hull centering
sv_rehlds_send_mapcycle 0                # Send mapcycle in serverinfo
sv_rehlds_local_gametime 0               # Local gametime feature
sv_rehlds_attachedentities_playeranimationspeed_fix 0  # Animation speed fix

# Resource Management
sv_rehlds_userinfo_transmitted_fields "" # Userinfo field filtering
```

### Advanced Features
```bash
# Entity Files
sv_use_entity_file 0                     # Use custom entity files (0=original, 1=use .ent, 2=create .ent)

# Random Seed
sv_usercmd_custom_random_seed 0          # Custom random seed

# Server Tags
sv_tags ""                               # Server tags for browser
```

## RCON Commands

### User Management
```bash
# RCON User Management
rcon_adduser <ipaddress/CIDR>            # Add RCON user
rcon_deluser <ipaddress> {removeAll}     # Remove RCON user
rcon_users                               # List RCON users

# RCON Security
sv_rcon_minfailures 5                    # Min RCON failures
sv_rcon_maxfailures 10                   # Max RCON failures
sv_rcon_minfailuretime 30                # RCON failure time window
sv_rcon_banpenalty 0                     # RCON ban penalty
```

### Resource Management
```bash
# Resource Commands
rescount                                 # Total precached resources
reslist <type>                           # List resources by type
# Types: sound, model, decal, generic, event
```

## Logging and Monitoring

### Logging Configuration
```bash
# Logging Settings
log on                                   # Enable logging
sv_logbans 1                             # Log bans
sv_logecho 1                             # Echo logs to console
sv_logfile 1                             # Write logs to file
sv_log_onefile 0                         # Single log file
sv_logsdir "logs"                        # Log directory

# Game Logging
mp_logecho 1                             # Echo game logs
mp_logfile 1                             # Write game logs
mp_logmessages 1                         # Log messages
mp_logdetail 3                           # Log detail level
```

### Performance Monitoring
```bash
# Server Statistics
sv_stats 1                               # Enable statistics
status                                   # Server status
users                                    # Connected users
```

## Network Protocol

### Client-Server Communication
```bash
# Protocol Settings
sv_protocol 48                           # Protocol version
sv_allowupload 1                         # Allow file uploads
sv_allowdownload 1                       # Allow file downloads
sv_uploadmax 0.5                         # Max upload rate
sv_downloadurl ""                        # Download URL
```

### Connection Management
```bash
# Connection Settings
sv_connect_timeout 30                    # Connection timeout
sv_connect_retry_time 5                  # Connection retry time
sv_connect_retry_count 3                 # Connection retry count
```

## Plugin System

### Metamod Support
```bash
# Metamod Configuration
meta list                                # List loaded plugins
meta info <plugin>                       # Plugin information
meta pause <plugin>                      # Pause plugin
meta unpause <plugin>                    # Unpause plugin
meta unload <plugin>                     # Unload plugin
meta reload <plugin>                     # Reload plugin
```

### AMX Mod X Support
```bash
# AMX Mod X Commands
amx_help                                 # AMX Mod X help
amx_plugins                              # List plugins
amx_modules                              # List modules
amx_reloadadmins                         # Reload admins
amx_reloadadmins                         # Reload admins
```

## Error Handling

### Common Error Codes
```bash
# Network Errors
NET_ERROR_TIMEOUT                        # Connection timeout
NET_ERROR_DISCONNECTED                   # Client disconnected
NET_ERROR_INVALID_PACKET                 # Invalid packet
NET_ERROR_RATE_LIMIT                     # Rate limit exceeded

# Server Errors
SERVER_ERROR_MEMORY                      # Memory allocation error
SERVER_ERROR_RESOURCE                    # Resource loading error
SERVER_ERROR_PLUGIN                      # Plugin error
```

## Performance Tuning

### Memory Management
```bash
# Memory Settings
sv_memory_limit 0                        # Memory limit (0=unlimited)
sv_memory_warning 80                     # Memory warning threshold
sv_memory_critical 95                    # Memory critical threshold
```

### CPU Optimization
```bash
# CPU Settings
sv_cpu_limit 0                           # CPU limit (0=unlimited)
sv_cpu_warning 80                        # CPU warning threshold
sv_cpu_critical 95                       # CPU critical threshold
```

## Security Best Practices

### Recommended Security Settings
```bash
# Essential Security CVARs
sv_rehlds_force_unmodified 1
sv_filterban 1
sv_rehlds_maxclients_from_single_ip 3
sv_rehlds_movecmdrate_max_avg 400
sv_rehlds_stringcmdrate_max_avg 80
sv_net_incoming_decompression 1
sv_net_incoming_decompression_max_ratio 80.0
sv_rcon_minfailures 5
sv_rcon_maxfailures 10
sv_rcon_minfailuretime 30
```

### Network Security
```bash
# Network Security Settings
sv_maxrate 25000
sv_minrate 0
sv_maxupdaterate 101
sv_minupdaterate 30
sv_timeout 65
cl_timeout 65
```

## Troubleshooting Commands

### Diagnostic Commands
```bash
# Server Diagnostics
version                                  # Server version
status                                   # Server status
users                                    # Connected users
stats                                    # Server statistics
ping                                     # Server ping

# Network Diagnostics
net_graph 1                              # Network graph
net_graphpos 1                           # Graph position
net_graphwidth 192                       # Graph width
```

### Debug Commands
```bash
# Debug Settings
sv_debug 0                               # Debug mode
sv_debug_net 0                           # Network debug
sv_debug_entities 0                      # Entity debug
sv_debug_physics 0                       # Physics debug
```

This API reference provides comprehensive coverage of ReHLDS CVARs, commands, and configuration options for Counter-Strike 1.6 server administration and optimization.
