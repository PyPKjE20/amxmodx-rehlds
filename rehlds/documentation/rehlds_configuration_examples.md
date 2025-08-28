# ReHLDS - Configuration Examples and Best Practices

## Server Startup Scripts

### Basic Counter-Strike 1.6 Server
```bash
#!/bin/bash
# Basic CS 1.6 server with ReHLDS

./hlds_run -game cstrike \
  +map de_dust2 \
  +maxplayers 32 \
  +port 27015 \
  +sv_lan 0 \
  +sv_region 3 \
  +hostname "My CS 1.6 Server" \
  +sv_password "" \
  +rcon_password "your_rcon_password"
```

### Advanced ReHLDS Configuration
```bash
#!/bin/bash
# Advanced ReHLDS server with security features

./hlds_run -game cstrike \
  +map de_dust2 \
  +maxplayers 32 \
  +port 27015 \
  +sv_lan 0 \
  +hostname "Secure CS 1.6 Server" \
  +sv_password "" \
  +rcon_password "secure_rcon_password" \
  +sv_rehlds_force_unmodified 1 \
  +sv_filterban 1 \
  +sv_rehlds_maxclients_from_single_ip 3 \
  +sv_rehlds_movecmdrate_max_avg 400 \
  +sv_rehlds_stringcmdrate_max_avg 80 \
  +sv_net_incoming_decompression 1 \
  +sv_net_incoming_decompression_max_ratio 80.0
```

## Server Configuration Files

### server.cfg
```bash
// ReHLDS Server Configuration
// Basic Settings
hostname "My ReHLDS CS 1.6 Server"
sv_password ""
rcon_password "your_secure_rcon_password"
sv_region 3
sv_lan 0

// Network Settings
sv_maxrate 25000
sv_minrate 0
sv_maxupdaterate 101
sv_minupdaterate 30
sv_maxspeed 320
sv_friction 4
sv_gravity 800
sv_airaccelerate 10
sv_wateraccelerate 10
sv_accelerate 5
sv_stopspeed 75
sv_stepsize 18
sv_waterdist 12
sv_waterfriction 1
sv_waterjump 0

// Game Settings
mp_timelimit 30
mp_roundtime 5
mp_freezetime 6
mp_buytime 1.5
mp_c4timer 35
mp_fraglimit 0
mp_maxrounds 0
mp_winlimit 0
mp_autoteambalance 1
mp_autokick 0
mp_flashlight 1
mp_footsteps 1
mp_limitteams 2
mp_teamlist "hgrunt;scientist"
mp_allowspectators 1
mp_chattime 10
mp_forcecamera 0
mp_friendlyfire 0
mp_friendly_grenade_damage 0
mp_limitteams 2
mp_logecho 1
mp_logfile 1
mp_logmessages 1
mp_logdetail 3
mp_tkpunish 0
mp_autokick 0
mp_autoteambalance 1
mp_roundtime 5
mp_timelimit 30
mp_freezetime 6
mp_buytime 1.5
mp_c4timer 35
mp_fraglimit 0
mp_maxrounds 0
mp_winlimit 0
mp_flashlight 1
mp_footsteps 1
mp_teamlist "hgrunt;scientist"
mp_allowspectators 1
mp_chattime 10
mp_forcecamera 0
mp_friendlyfire 0
mp_friendly_grenade_damage 0
mp_limitteams 2
mp_logecho 1
mp_logfile 1
mp_logmessages 1
mp_logdetail 3
mp_tkpunish 0

// ReHLDS Security Settings
sv_rehlds_force_unmodified 1
sv_filterban 1
sv_rehlds_maxclients_from_single_ip 3
sv_rehlds_movecmdrate_max_avg 400
sv_rehlds_movecmdrate_avg_punish 5
sv_rehlds_movecmdrate_max_burst 2500
sv_rehlds_movecmdrate_burst_punish 5
sv_rehlds_stringcmdrate_max_avg 80
sv_rehlds_stringcmdrate_avg_punish 5
sv_rehlds_stringcmdrate_max_burst 400
sv_rehlds_stringcmdrate_burst_punish 5
sv_net_incoming_decompression 1
sv_net_incoming_decompression_max_ratio 80.0
sv_net_incoming_decompression_max_size 65536
sv_net_incoming_decompression_min_failures 4
sv_net_incoming_decompression_max_failures 10
sv_net_incoming_decompression_min_failuretime 0.1
sv_net_incoming_decompression_punish -1

// ReHLDS Performance Settings
sv_rehlds_maxpacket_loss 0
sv_rehlds_hull_centering 0
sv_rehlds_send_mapcycle 0
sv_rehlds_local_gametime 0
sv_rehlds_attachedentities_playeranimationspeed_fix 0

// ReHLDS Advanced Features
sv_use_entity_file 0
sv_usercmd_custom_random_seed 0
sv_rehlds_userinfo_transmitted_fields "\name\model\*sid\*hltv\bottomcolor\topcolor\rate\cl_updaterate\cl_cmdrate\cl_dlmax\cl_righthand\cl_observercrosshair\*ip\*uid"

// Logging Settings
log on
sv_logbans 1
sv_logecho 1
sv_logfile 1
sv_log_onefile 0
sv_logsdir "logs"

// RCON Settings
rcon_password "your_secure_rcon_password"
sv_rcon_minfailures 5
sv_rcon_maxfailures 10
sv_rcon_minfailuretime 30
sv_rcon_banpenalty 0
sv_rcon_minfailures 5
sv_rcon_maxfailures 10
sv_rcon_minfailuretime 30
sv_rcon_banpenalty 0

// Tags for server browser
sv_tags "rehlds,secure,competitive"
```

### mapcycle.txt
```
de_dust2
de_inferno
de_nuke
de_train
de_mirage
de_overpass
de_cache
de_cobblestone
de_ancient
de_vertigo
```

### motd.txt
```
<html>
<head>
<title>Welcome to My CS 1.6 Server</title>
</head>
<body bgcolor="#000000" text="#FFFFFF">
<center>
<h1>Welcome to My Counter-Strike 1.6 Server</h1>
<p>Powered by ReHLDS - Enhanced Half-Life Dedicated Server</p>
<br>
<h2>Server Features:</h2>
<ul>
<li>Enhanced Security</li>
<li>Anti-Cheat Protection</li>
<li>Optimized Performance</li>
<li>Stable Gameplay</li>
</ul>
<br>
<p>Enjoy your game!</p>
</center>
</body>
</html>
```

## Performance Optimization

### High-Performance Configuration
```bash
# CPU and Memory Optimization
sv_maxrate 25000
sv_minrate 0
sv_maxupdaterate 101
sv_minupdaterate 30
sv_maxspeed 320
sv_friction 4
sv_gravity 800
sv_airaccelerate 10
sv_wateraccelerate 10
sv_accelerate 5
sv_stopspeed 75
sv_stepsize 18
sv_waterdist 12
sv_waterfriction 1
sv_waterjump 0

# Network Optimization
sv_rehlds_maxpacket_loss 0
sv_rehlds_hull_centering 0
sv_rehlds_send_mapcycle 0
sv_rehlds_local_gametime 0

# Resource Management
sv_rehlds_attachedentities_playeranimationspeed_fix 0
sv_use_entity_file 0
sv_usercmd_custom_random_seed 0
```

### Security-Focused Configuration
```bash
# Anti-Cheat and Security
sv_rehlds_force_unmodified 1
sv_filterban 1
sv_rehlds_maxclients_from_single_ip 3

# Rate Limiting
sv_rehlds_movecmdrate_max_avg 400
sv_rehlds_movecmdrate_avg_punish 5
sv_rehlds_movecmdrate_max_burst 2500
sv_rehlds_movecmdrate_burst_punish 5
sv_rehlds_stringcmdrate_max_avg 80
sv_rehlds_stringcmdrate_avg_punish 5
sv_rehlds_stringcmdrate_max_burst 400
sv_rehlds_stringcmdrate_burst_punish 5

# Decompression Protection
sv_net_incoming_decompression 1
sv_net_incoming_decompression_max_ratio 80.0
sv_net_incoming_decompression_max_size 65536
sv_net_incoming_decompression_min_failures 4
sv_net_incoming_decompression_max_failures 10
sv_net_incoming_decompression_min_failuretime 0.1
sv_net_incoming_decompression_punish -1

# RCON Security
sv_rcon_minfailures 5
sv_rcon_maxfailures 10
sv_rcon_minfailuretime 30
sv_rcon_banpenalty 0
```

## Monitoring and Logging

### Logging Configuration
```bash
# Enable comprehensive logging
log on
sv_logbans 1
sv_logecho 1
sv_logfile 1
sv_log_onefile 0
sv_logsdir "logs"

# Game event logging
mp_logecho 1
mp_logfile 1
mp_logmessages 1
mp_logdetail 3

# Performance monitoring
sv_stats 1
```

### Resource Monitoring Commands
```bash
# Monitor server resources
rescount                    # Total precached resources
reslist sound              # Sound resources
reslist model              # Model resources
reslist decal              # Decal resources
reslist generic            # Generic resources
reslist event              # Event resources

# Monitor server performance
sv_stats                   # Server statistics
status                     # Player status
users                      # Connected users
```

## Troubleshooting

### Common Issues and Solutions

#### Server Won't Start
```bash
# Check file permissions
chmod +x hlds_run
chmod +x hlds_linux

# Check dependencies
ldd hlds_linux

# Check port availability
netstat -tulpn | grep 27015
```

#### Performance Issues
```bash
# Monitor CPU and memory usage
top -p $(pgrep hlds_linux)
htop -p $(pgrep hlds_linux)

# Check network usage
iftop -i eth0
nethogs eth0
```

#### Connection Problems
```bash
# Test network connectivity
ping -c 4 8.8.8.8
traceroute 8.8.8.8

# Check firewall settings
iptables -L
ufw status
```

## Plugin Integration

### Metamod Installation
```bash
# Download Metamod
wget https://github.com/alliedmodders/metamod-p/releases/download/1.21.1/metamod-p-1.21.1-linux.tar.gz
tar -xzf metamod-p-1.21.1-linux.tar.gz

# Install Metamod
cp metamod-p-1.21.1/linux/addons/metamod.so cstrike/addons/metamod/dlls/

# Configure Metamod
echo "linux addons/metamod/dlls/metamod.so" > cstrike/addons/metamod/plugins.ini
```

### AMX Mod X Installation
```bash
# Download AMX Mod X
wget https://github.com/alliedmodders/amxmodx/releases/download/1.10.0/amxmodx-1.10.0-base-linux.tar.gz
tar -xzf amxmodx-1.10.0-base-linux.tar.gz

# Install AMX Mod X
cp amxmodx-1.10.0/addons/amxmodx/dlls/amxmodx_mm_i386.so cstrike/addons/amxmodx/dlls/

# Configure AMX Mod X
echo "linux addons/amxmodx/dlls/amxmodx_mm_i386.so" >> cstrike/addons/metamod/plugins.ini
```

## Backup and Maintenance

### Backup Script
```bash
#!/bin/bash
# ReHLDS Server Backup Script

BACKUP_DIR="/backup/rehlds"
SERVER_DIR="/home/cs16"
DATE=$(date +%Y%m%d_%H%M%S)

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup server configuration
tar -czf $BACKUP_DIR/server_config_$DATE.tar.gz \
  $SERVER_DIR/server.cfg \
  $SERVER_DIR/mapcycle.txt \
  $SERVER_DIR/motd.txt \
  $SERVER_DIR/listenserver.cfg

# Backup logs
tar -czf $BACKUP_DIR/logs_$DATE.tar.gz $SERVER_DIR/logs/

# Backup addons
tar -czf $BACKUP_DIR/addons_$DATE.tar.gz $SERVER_DIR/cstrike/addons/

# Clean old backups (keep last 7 days)
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete

echo "Backup completed: $DATE"
```

### Maintenance Script
```bash
#!/bin/bash
# ReHLDS Server Maintenance Script

SERVER_DIR="/home/cs16"
LOG_DIR="$SERVER_DIR/logs"

# Rotate logs
if [ -d "$LOG_DIR" ]; then
    find $LOG_DIR -name "*.log" -mtime +7 -delete
    find $LOG_DIR -name "*.txt" -mtime +30 -delete
fi

# Clean temporary files
find $SERVER_DIR -name "*.tmp" -delete
find $SERVER_DIR -name "*.cache" -delete

# Check disk space
DISK_USAGE=$(df $SERVER_DIR | tail -1 | awk '{print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 90 ]; then
    echo "Warning: Disk usage is $DISK_USAGE%"
fi

# Restart server if needed
if ! pgrep -f hlds_linux > /dev/null; then
    echo "Server not running, restarting..."
    cd $SERVER_DIR
    ./start_server.sh &
fi

echo "Maintenance completed: $(date)"
```

These configuration examples provide comprehensive coverage of ReHLDS server setup, optimization, security, and maintenance for Counter-Strike 1.6 servers.
