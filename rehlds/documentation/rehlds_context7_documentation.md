# ReHLDS - Context7 Documentation

## Overview
ReHLDS (Reverse-engineered HLDS) is an enhanced version of the Half-Life Dedicated Server engine with bug fixes, security patches, and optimizations for stable Counter-Strike 1.6 servers. It is a result of reverse engineering the original HLDS (build 6152/6153) and provides improved stability, security, and performance for Half-Life 1 game servers.

## Key Features
- **Enhanced Stability**: Bug fixes and improvements over original HLDS
- **Security Patches**: Protection against common exploits and vulnerabilities
- **Performance Optimizations**: Better resource usage and server performance
- **Extended Configuration**: Additional CVARs for fine-tuning server behavior
- **Anti-Cheat Features**: Built-in protection against common cheating methods
- **Network Improvements**: Better packet handling and connection management
- **Resource Management**: Enhanced precache limits and resource handling

## Repository Structure
```
rehlds/
├── rehlds/              # Core ReHLDS source code
├── msvc/                # Visual Studio project files
├── dep/                 # Dependencies and third-party libraries
├── .github/workflows/   # CI/CD configuration
├── CMakeLists.txt       # CMake build configuration
├── build.sh             # Linux build script
└── README.md            # Project documentation
```

## Core Components

### 1. Engine Core (rehlds/)
- **Server Engine**: Main server logic and game engine
- **Network Layer**: Client-server communication handling
- **Entity System**: Game object management and manipulation
- **Physics Engine**: Collision detection and movement
- **Memory Management**: Resource allocation and cleanup
- **Plugin System**: Metamod and AMX Mod X support

### 2. Build System
- **Visual Studio**: Windows development with MSVC
- **CMake**: Cross-platform build configuration
- **GCC/Clang**: Linux compilation support
- **Multi-compiler**: Support for Intel ICC, GCC, and Clang

### 3. Configuration System
- **CVAR Management**: Server configuration variables
- **Network Settings**: Connection and bandwidth optimization
- **Security Options**: Anti-cheat and protection features
- **Performance Tuning**: Resource and memory management

## Development Workflow

### 1. Building from Source
```bash
# Linux build
./build.sh --compiler=gcc --jobs=4

# Windows build (Visual Studio)
# Open msvc/ReHLDS.sln and build Release Swds configuration
```

### 2. Server Configuration
```bash
# Basic server startup
./hlds_run -game cstrike +map de_dust2 +maxplayers 32

# With ReHLDS specific settings
./hlds_run -game cstrike +map de_dust2 +maxplayers 32 +sv_rehlds_force_unmodified 1
```

### 3. Plugin Integration
```bash
# Metamod installation
# Copy metamod.dll to cstrike/dlls/

# AMX Mod X installation
# Copy amxmodx_mm.dll to cstrike/addons/amxmodx/dlls/
```

## Key CVARs and Configuration

### Security and Anti-Cheat
```bash
# Force unmodified files
sv_rehlds_force_unmodified 1

# Packet filtering
sv_filterban 1

# Rate limiting
sv_rehlds_movecmdrate_max_avg 400
sv_rehlds_stringcmdrate_max_avg 80

# Decompression protection
sv_net_incoming_decompression 1
sv_net_incoming_decompression_max_ratio 80.0
```

### Performance Optimization
```bash
# Network optimization
sv_rehlds_maxpacket_loss 0
sv_rehlds_hull_centering 0

# Resource management
sv_rehlds_send_mapcycle 0

# Local gametime feature
sv_rehlds_local_gametime 0
```

### Advanced Features
```bash
# Custom entity files
sv_use_entity_file 1

# Custom random seed
sv_usercmd_custom_random_seed 0

# Userinfo field filtering
sv_rehlds_userinfo_transmitted_fields "\name\model\*sid\*hltv\bottomcolor\topcolor"
```

## Plugin Development Support

### Metamod Integration
```cpp
// Metamod plugin structure
#include <metamod.h>

// Plugin information
META_API_HOOKS
META_API_HOOKS_END

// Plugin functions
void meta_init() {
    // Plugin initialization
}

void meta_attach() {
    // Plugin attachment
}

void meta_detach() {
    // Plugin detachment
}
```

### AMX Mod X Compatibility
```pawn
// AMX Mod X plugin structure
#include <amxmodx>

public plugin_init() {
    register_plugin("ReHLDS Plugin", "1.0", "Author")
    // Plugin functionality
}
```

## Network and Protocol

### Client-Server Communication
- **Protocol Version**: Compatible with Half-Life 1 protocol
- **Packet Handling**: Enhanced packet validation and processing
- **Rate Limiting**: Configurable command rate limits
- **Compression**: Built-in bzip2 decompression with protection

### Connection Management
```bash
# Connection limits
sv_rehlds_maxclients_from_single_ip 5

# Timeout settings
sv_timeout 65
cl_timeout 65

# Network optimization
sv_maxrate 25000
sv_minrate 0
```

## Security Features

### Anti-Exploit Protection
- **Packet Validation**: Enhanced packet structure validation
- **Command Rate Limiting**: Protection against command flooding
- **Decompression Protection**: Anti-compression bomb protection
- **Userinfo Filtering**: Controlled data transmission

### Ban and Filter System
```bash
# IP filtering
rcon_adduser <ipaddress/CIDR>
rcon_deluser <ipaddress>

# Packet filtering
sv_filterban 1  # Enable IP ban checking
```

## Performance Monitoring

### Resource Management
```bash
# Resource counting
rescount  # Total precached resources
reslist sound  # Sound resources
reslist model  # Model resources
reslist decal  # Decal resources
```

### Server Optimization
- **Memory Management**: Improved memory allocation and cleanup
- **CPU Optimization**: Better threading and processing
- **Network Efficiency**: Optimized packet handling
- **Resource Caching**: Enhanced precache system

## Debugging and Troubleshooting

### Logging and Monitoring
```bash
# Enable detailed logging
log on
sv_logbans 1
sv_logecho 1
sv_logfile 1

# Monitor server performance
sv_stats 1
```

### Common Issues
- **Memory Leaks**: Improved memory management prevents leaks
- **Network Issues**: Enhanced packet handling reduces disconnections
- **Plugin Conflicts**: Better plugin system compatibility
- **Performance Problems**: Optimized engine reduces lag

## Integration with Game Mods

### Counter-Strike 1.6
- **Full Compatibility**: Complete CS 1.6 support
- **Enhanced Stability**: Better performance for CS servers
- **Anti-Cheat**: Built-in protection against common CS cheats
- **Plugin Support**: Full Metamod and AMX Mod X compatibility

### Other Half-Life 1 Mods
- **Day of Defeat**: Full compatibility
- **Team Fortress Classic**: Supported
- **Natural Selection**: Compatible
- **Custom Mods**: Extensible architecture

## Best Practices

### Server Administration
1. **Regular Updates**: Keep ReHLDS updated to latest version
2. **Configuration Backup**: Backup server configurations
3. **Monitoring**: Use built-in monitoring tools
4. **Security**: Enable all security features
5. **Performance Tuning**: Optimize CVARs for your setup

### Plugin Development
1. **Compatibility Testing**: Test plugins with ReHLDS
2. **Resource Management**: Use efficient resource handling
3. **Error Handling**: Implement proper error checking
4. **Performance**: Optimize plugin performance
5. **Security**: Follow security best practices

## Version Information
- **Current Version**: 3.14.0.857
- **License**: MIT
- **Supported Platforms**: Windows, Linux
- **Supported Games**: Half-Life 1, Counter-Strike 1.6, Day of Defeat, Team Fortress Classic
- **Development Status**: Active maintenance

## Resources and Documentation

### Official Resources
- **Repository**: https://github.com/rehlds/rehlds
- **Documentation**: https://rehlds.dev/docs/rehlds
- **Website**: https://rehlds.dev
- **Releases**: https://github.com/rehlds/rehlds/releases

### Community
- **GitHub Issues**: Bug reports and feature requests
- **Discussions**: Community support and development
- **Wiki**: Additional documentation and guides

## Context7 Integration Notes
This documentation provides comprehensive coverage of ReHLDS for Context7 integration, including:
- Core functionality and architecture
- Server administration and configuration
- Plugin development support
- Security and performance features
- Troubleshooting and best practices
- Integration with game mods and plugins

The information is structured to support server administrators and developers working with ReHLDS, providing both high-level overview and specific implementation details for Counter-Strike 1.6 server management.
