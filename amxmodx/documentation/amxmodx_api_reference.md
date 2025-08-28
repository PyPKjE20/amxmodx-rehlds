# AMX Mod X - API Reference

## Core Natives

### Plugin Management
```pawn
// Register plugin information
native register_plugin(const name[], const version[], const author[])

// Plugin return values
#define PLUGIN_CONTINUE    0
#define PLUGIN_HANDLED     1
#define PLUGIN_STOP        2

// Plugin lifecycle
public plugin_init()      // Called when plugin loads
public plugin_cfg()       // Called after config files load
public plugin_end()       // Called when plugin unloads
public plugin_natives()   // Called to register custom natives
```

### Command Registration
```pawn
// Register client commands (chat commands)
native register_clcmd(const command[], const function[], const flags = 0, const info[] = "", const flag = 0)

// Register console commands
native register_concmd(const command[], const function[], const level, const info[] = "")

// Register events
native register_event(const event[], const function[], const flags, const cond[] = "")

// Register log events
native register_logevent(const function[], const numargs, const event[])

// Register forward events
native register_forward(const forward, const function[], const post = 0)
```

### Player Functions
```pawn
// Player information
native get_user_name(const id, name[], len)
native get_user_authid(const id, authid[], len)
native get_user_ip(const id, ip[], len, const without_port = 0)
native get_user_userid(const id)
native get_user_team(const id)
native get_user_deaths(const id)
native get_user_frags(const id)
native get_user_health(const id)
native get_user_armor(const id)
native get_user_flags(const id)
native get_user_rendering(const id, &renderfx, &rendermode, &renderamt, &rendercolor[3])

// Player state
native is_user_alive(const id)
native is_user_connected(const id)
native is_user_bot(const id)
native is_user_hltv(const id)
native is_user_admin(const id)

// Player modification
native set_user_health(const id, const health)
native set_user_armor(const id, const armor)
native set_user_frags(const id, const frags)
native set_user_deaths(const id, const deaths)
native set_user_rendering(const id, const renderfx, const rendermode, const renderamt, const rendercolor[3])
native set_user_flags(const id, const flags, const bRemove = 0)
```

### Entity Functions
```pawn
// Entity creation and destruction
native create_entity(const classname[])
native remove_entity(const entity)
native entity_set_origin(const entity, const Float:origin[3])
native entity_get_origin(const entity, Float:origin[3])

// Entity properties
native entity_set_string(const entity, const item, const value[])
native entity_get_string(const entity, const item, value[], const maxlen)
native entity_set_int(const entity, const item, const value)
native entity_get_int(const entity, const item)
native entity_set_float(const entity, const item, const Float:value)
native entity_get_float(const entity, const item)

// Entity finding
native find_ent_by_class(const start_from, const classname[])
native find_ent_by_owner(const start_from, const classname[], const owner)
native find_ent_by_target(const start_from, const target[])
native find_ent_by_model(const start_from, const model[])
native find_ent_by_tname(const start_from, const targetname[])
native find_ent_in_sphere(const start_from, const Float:origin[3], const Float:radius)
```

### Message Functions
```pawn
// Client messages
native client_print(const id, const type, const input[], any:...)
native show_activity(const id, const name[], const input[], any:...)
native show_activity_id(const id, const target, const name[], const input[], any:...)

// Hud messages
native show_hudmessage(const id, const channel, const message[], any:...)
native set_hudmessage(const red = 255, const green = 255, const blue = 255, const Float:x = -1.0, const Float:y = 0.35, const effects = 0, const Float:fxtime = 6.0, const Float:holdtime = 12.0, const Float:fadeintime = 0.1, const Float:fadeouttime = 0.2, const channel = 4)

// Menu system
native menu_create(const title[], const handler)
native menu_additem(const menu, const name[], const data[] = "")
native menu_addblank(const menu, const pagenum = 0)
native menu_addtext(const menu, const text[], const pagenum = 0)
native menu_display(const id, const menu, const page = 0)
native menu_destroy(const menu)
```

### Utility Functions
```pawn
// String functions
native copy(const dest[], const source[], const maxlen)
native equal(const string1[], const string2[], const case_sensitive = 0)
native contain(const string[], const substring[])
native containi(const string[], const substring[])
native replace(const string[], const maxlen, const what[], const with[])
native replace_all(const string[], const maxlen, const what[], const with[])
native trim(const string[])
native str_to_num(const string[])
native num_to_str(const num, string[], const maxlen)
native str_to_float(const string[])
native float_to_str(const Float:num, string[], const maxlen)

// Array functions
native copy(const dest[], const source[], const maxlen)
native equal(const array1[], const array2[], const size)
native fill(const array[], const size, const value)

// Math functions
native floatround(const Float:num, const type = floatround_round)
native floatabs(const Float:num)
native floatsquare(const Float:num)
native floatpower(const Float:base, const Float:exponent)
native floatlog(const Float:num, const Float:base = 10.0)
native floatsin(const Float:angle, const type = radian)
native floatcos(const Float:angle, const type = radian)
native floattan(const Float:angle, const type = radian)
```

### File Operations
```pawn
// File handling
native fopen(const filename[], const mode[])
native fclose(const file)
native fgets(const file, text[], const maxlen)
native fputs(const file, const text[])
native feof(const file)
native file_size(const filename[])
native file_exists(const filename[])
native delete_file(const filename[])
native rename_file(const oldname[], const newname[])
native copy_file(const source[], const dest[])
```

### Timer Functions
```pawn
// Task management
native set_task(const Float:time, const function[], const id = 0, const parameter[] = "", const len = 0, const flags = 0, const repeat = 0)
native remove_task(const id, const taskid = 0)
native change_task(const id, const Float:time, const parameter[] = "", const len = 0, const flags = 0, const repeat = 0)
native task_exists(const id, const taskid = 0)

// Timer flags
#define TASK_ONCE          0
#define TASK_REPEAT        1
#define TASK_REMOVE        2
#define TASK_DONTREMOVE    4
```

### Database Functions (SQL)
```pawn
// Database connection
native SQL_MakeDbTuple(const host[], const user[], const pass[], const db[])
native SQL_FreeHandle(const Handle:tuple)
native SQL_Connect(const Handle:tuple, &Handle:connection, &error[], const maxlen)

// Query execution
native SQL_Query(const Handle:connection, const query[], const data[] = "", const dataSize = 0)
native SQL_ThreadQuery(const Handle:tuple, const query[], const callback[], const data[] = "", const dataSize = 0)
native SQL_PrepareQuery(const Handle:connection, const query[], const data[] = "", const dataSize = 0)

// Result handling
native SQL_NumResults(const Handle:query)
native SQL_NumColumns(const Handle:query)
native SQL_ReadResult(const Handle:query, const column, const type = SQLTYPE_INT)
native SQL_ReadResultString(const Handle:query, const column, string[], const maxlen)
native SQL_MoreResults(const Handle:query)
native SQL_NextRow(const Handle:query)
native SQL_FreeHandle(const Handle:query)

// String escaping
native SQL_QuoteString(const Handle:connection, const string[], const maxlen, const source[])
```

### CVAR Functions
```pawn
// CVAR management
native register_cvar(const name[], const string[], const flags = 0, const Float:fvalue = 0.0)
native get_cvar_pointer(const name[])
native get_cvar_string(const name[], string[], const maxlen)
native get_cvar_num(const name[])
native get_cvar_float(const name[])
native set_cvar_string(const name[], const string[])
native set_cvar_num(const name[], const value)
native set_cvar_float(const name[], const Float:value)
native hook_cvar_change(const cvar, const function[])
native unhook_cvar_change(const cvar, const function[])

// CVAR flags
#define FCVAR_ARCHIVE      1
#define FCVAR_USERINFO     2
#define FCVAR_SERVER       4
#define FCVAR_EXTDLL       8
#define FCVAR_CLIENTDLL    16
#define FCVAR_PROTECTED    32
#define FCVAR_SPONLY       64
#define FCVAR_PRINTABLEONLY 128
#define FCVAR_UNLOGGED     256
#define FCVAR_NOEXTRAWHITEPACE 512
```

### Logging Functions
```pawn
// Logging
native log_amx(const input[], any:...)
native log_message(const input[], any:...)
native log_to_file(const filename[], const input[], any:...)
native log_to_file_ex(const filename[], const input[], any:...)
native log_amx_ex(const input[], any:...)
native log_message_ex(const input[], any:...)
native log_to_file_ex(const filename[], const input[], any:...)
```

## Event System

### Game Events
```pawn
// Player events
public client_putinserver(id)           // Player connects
public client_disconnect(id)            // Player disconnects
public client_death(killer, victim, weapon, hitplace)  // Player death
public client_spawn(id)                 // Player spawns
public client_team(id, team)            // Player changes team
public client_authorized(id)            // Player authorized
public client_prethink(id)              // Before player think
public client_postthink(id)             // After player think

// Weapon events
public client_weaponpick(id, weapon)    // Player picks up weapon
public client_weaponremove(id, weapon)  // Player drops weapon

// Map events
public server_frame()                   // Server frame
public server_spawn()                   // Server spawn
public map_start()                      // Map starts
public map_end()                        // Map ends
```

### Forward Events
```pawn
// Player forwards
#define FORWARD_PLAYER_CONNECT          0
#define FORWARD_PLAYER_DISCONNECT       1
#define FORWARD_PLAYER_SPAWN            2
#define FORWARD_PLAYER_DEATH            3
#define FORWARD_PLAYER_TEAM             4
#define FORWARD_PLAYER_AUTHORIZED       5

// Server forwards
#define FORWARD_SERVER_FRAME            6
#define FORWARD_SERVER_SPAWN            7
#define FORWARD_MAP_START               8
#define FORWARD_MAP_END                 9
```

## Module-Specific Natives

### Fun Module
```pawn
// Player effects
native set_user_gravity(const id, const Float:gravity)
native get_user_gravity(const id)
native set_user_maxspeed(const id, const Float:speed)
native get_user_maxspeed(const id)
native set_user_velocity(const id, const Float:velocity[3])
native get_user_velocity(const id, Float:velocity[3])
native set_user_punchangle(const id, const Float:punchangle[3])
native get_user_punchangle(const id, Float:punchangle[3])

// Player actions
native give_item(const id, const item[])
native strip_user_weapons(const id)
native set_user_ammo(const id, const weapon, const ammo)
native get_user_ammo(const id, const weapon)
native set_user_bpammo(const id, const weapon, const ammo)
native get_user_bpammo(const id, const weapon)

// Teleportation
native set_user_origin(const id, const Float:origin[3])
native get_user_origin(const id, Float:origin[3])
native teleport(const id, const Float:origin[3], const Float:angles[3], const Float:velocity[3])
```

### Engine Module
```pawn
// Engine functions
native get_entity_flags(const entity)
native set_entity_flags(const entity, const flags)
native get_entity_edict(const entity)
native set_entity_edict(const entity, const edict)
native get_entity_distance(const entity1, const entity2)
native get_entity_distance_float(const Float:origin1[3], const Float:origin2[3])

// Trace functions
native trace_line(const Float:start[3], const Float:end[3], const ignore_monsters, const ignore_glass, const id, &tr)
native trace_hull(const Float:start[3], const Float:end[3], const ignore_monsters, const hull, const id, &tr)
native trace_result(const tr, const type)
native trace_get_entity(const tr)
native trace_get_fraction(const tr)
native trace_get_endpos(const tr, Float:endpos[3])
```

### Fakemeta Module
```pawn
// Entity manipulation
native engfunc(EngFunc:function, any:...)
native dllfunc(DLLFunc:function, any:...)
native pev(const entity, const offset, any:...)
native set_pev(const entity, const offset, any:...)
native fm_entity_get_edict(const entity)
native fm_entity_set_edict(const entity, const edict)
native fm_entity_get_int(const entity, const offset)
native fm_entity_set_int(const entity, const offset, const value)
native fm_entity_get_float(const entity, const offset)
native fm_entity_set_float(const entity, const offset, const Float:value)
native fm_entity_get_string(const entity, const offset, string[], const maxlen)
native fm_entity_set_string(const entity, const offset, const string[])
```

## Constants and Definitions

### Player Flags
```pawn
#define FLAG_KICK          1
#define FLAG_BAN           2
#define FLAG_SLAY          4
#define FLAG_MAP           8
#define FLAG_CVAR          16
#define FLAG_LEVEL_A       32
#define FLAG_LEVEL_B       64
#define FLAG_LEVEL_C       128
#define FLAG_LEVEL_D       256
#define FLAG_LEVEL_E       512
#define FLAG_LEVEL_F       1024
#define FLAG_LEVEL_G       2048
#define FLAG_LEVEL_H       4096
#define FLAG_IMMUNITY      32768
```

### Team Constants
```pawn
#define TEAM_UNASSIGNED    0
#define TEAM_TERRORIST     1
#define TEAM_CT            2
#define TEAM_SPECTATOR     3
```

### Message Types
```pawn
#define print_console      0
#define print_center       1
#define print_chat         2
#define print_team_red     3
#define print_team_blue    4
```

### Entity Flags
```pawn
#define FL_FLY             (1<<0)
#define FL_SWIM            (1<<1)
#define FL_CONVEYOR        (1<<2)
#define FL_CLIENT          (1<<3)
#define FL_INWATER         (1<<4)
#define FL_MONSTER         (1<<5)
#define FL_GODMODE         (1<<6)
#define FL_NOTARGET        (1<<7)
#define FL_SKIPLOCALHOST   (1<<8)
#define FL_ONGROUND        (1<<9)
#define FL_PARTIALGROUND   (1<<10)
#define FL_WATERJUMP       (1<<11)
#define FL_FROZEN          (1<<12)
#define FL_FAKECLIENT      (1<<13)
#define FL_DUCKING         (1<<14)
#define FL_FLOAT           (1<<15)
#define FL_GRAPHED         (1<<16)
#define FL_IMMUNE_WATER    (1<<17)
#define FL_IMMUNE_SLIME    (1<<18)
#define FL_IMMUNE_LAVA     (1<<19)
#define FL_INVULNERABLE    (1<<20)
#define FL_SPECTATOR       (1<<21)
#define FL_ALWAYSTHINK     (1<<22)
#define FL_MONSTERCLIP     (1<<23)
#define FL_VEHICLE         (1<<24)
#define FL_PUSHSTEP        (1<<25)
#define FL_ONTRAIN         (1<<26)
#define FL_WORLDBRUSH      (1<<27)
#define FL_SPIDER          (1<<28)
#define FL_CYCLE           (1<<29)
#define FL_SWIM            (1<<30)
#define FL_CONVEYOR        (1<<31)
```

### Movement Types
```pawn
#define MOVETYPE_NONE              0
#define MOVETYPE_ANGLENOCLIP       1
#define MOVETYPE_ANGLECLIP         2
#define MOVETYPE_WALK              3
#define MOVETYPE_STEP              4
#define MOVETYPE_FLY               5
#define MOVETYPE_TOSS              6
#define MOVETYPE_PUSH              7
#define MOVETYPE_NOCLIP            8
#define MOVETYPE_FLYMISSILE        9
#define MOVETYPE_BOUNCE            10
#define MOVETYPE_BOUNCEMISSILE     11
#define MOVETYPE_FOLLOW            12
#define MOVETYPE_PUSHSTEP          13
```

### Solid Types
```pawn
#define SOLID_NOT           0
#define SOLID_TRIGGER       1
#define SOLID_BBOX          2
#define SOLID_SLIDEBOX      3
#define SOLID_BSP           4
```

This API reference provides comprehensive coverage of AMX Mod X native functions, constants, and event system. It serves as a quick reference for developers working with AMX Mod X plugin development.
