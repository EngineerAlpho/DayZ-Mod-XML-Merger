# DayZ Mod XML Automator

Automates the process of adding mod items and vehicles to DayZ server XML configuration files (`types.xml`, `events.xml`, and `spawnabletypes.xml`).

## Features

✅ Automatically adds items to `types.xml` with proper spawn configurations
✅ Creates vehicle spawn events in `events.xml` 
✅ Prevents duplicate entries
✅ Automatic backups before modifications
✅ Customizable default values for weapons, vehicles, and items
✅ Interactive mode or JSON file input
✅ Validates and preserves XML structure

## Installation

1. Install Python 3.6 or higher
2. No additional dependencies needed (uses built-in libraries)
3. Place the script in your DayZ server directory or anywhere accessible

## Quick Start

### First Time Setup

1. Run the script:
   ```bash
   python dayz_mod_xml_automator.py
   ```

2. On first run, it creates `mod_config.json` with default settings

3. **IMPORTANT**: Edit `mod_config.json` to match your server paths:
   ```json
   {
       "xml_paths": {
           "types": "./mpmissions/dayzOffline.chernarusplus/db/types.xml",
           "events": "./mpmissions/dayzOffline.chernarusplus/cfgeventspawns.xml",
           "spawnabletypes": "./mpmissions/dayzOffline.chernarusplus/db/spawnabletypes.xml"
       }
   }
   ```

### Usage Method 1: Interactive Mode (Easiest)

```bash
python dayz_mod_xml_automator.py
```

Select option 1, then:
- Choose item type (weapons/vehicles/items/all)
- Enter classnames one by one
- Press Enter on empty line when done
- Review and confirm

Example:
```
Enter weapon classnames (one per line, empty line to finish):
Weapon: ExpansionAK74
Weapon: ExpansionAWM
Weapon: [press Enter]
```

### Usage Method 2: JSON File (Best for Batches)

1. Create a JSON file with your items:
   ```json
   {
       "weapons": [
           "ExpansionAK74",
           "ExpansionM16A4"
       ],
       "vehicles": [
           "ExpansionUAZ",
           "ExpansionBus"
       ],
       "items": [
           "ExpansionAmmobox762x39"
       ]
   }
   ```

2. Run:
   ```bash
   python dayz_mod_xml_automator.py
   ```

3. Select option 2 and enter your JSON file path

## Configuration File (`mod_config.json`)

### Key Settings

**XML Paths** - Update these to match your server:
```json
"xml_paths": {
    "types": "path/to/your/types.xml",
    "events": "path/to/your/cfgeventspawns.xml",
    "spawnabletypes": "path/to/your/spawnabletypes.xml"
}
```

**Default Values for Weapons:**
```json
"weapons": {
    "nominal": 10,      // Max items in world
    "min": 5,           // Min items before respawn
    "lifetime": 3600,   // Seconds before cleanup (1 hour)
    "restock": 1800,    // Seconds between restocks (30 min)
    "cost": 100,        // Economy cost
    "usage": ["Military", "Police"]  // Where they spawn
}
```

**Default Values for Vehicles:**
```json
"vehicles": {
    "nominal": 3,
    "min": 1,
    "lifetime": 3888000,  // 45 days
    "restock": 0,         // Don't restock
    "usage": ["Industrial", "Farm"]
}
```

**Vehicle Events:**
```json
"vehicle_events": {
    "enabled": true,
    "default_event": {
        "nominal": 2,
        "min": 1,
        "max": 3,
        "saferadius": 500,
        "distanceradius": 500
    }
}
```

### Customization Examples

**Make weapons more rare:**
```json
"weapons": {
    "nominal": 5,
    "min": 2,
    "lifetime": 7200,
    "usage": ["Military"]
}
```

**Make vehicles more common:**
```json
"vehicles": {
    "nominal": 10,
    "min": 5
}
```

**Disable vehicle events (only spawn as loot):**
```json
"vehicle_events": {
    "enabled": false
}
```

## How It Works

### types.xml Entries
Adds items with proper structure:
```xml
<type name="ExpansionAK74">
    <nominal>10</nominal>
    <lifetime>3600</lifetime>
    <restock>1800</restock>
    <min>5</min>
    <flags count_in_cargo="1" count_in_map="1" count_in_player="1"/>
    <category name="weapons"/>
    <usage name="Military"/>
    <usage name="Police"/>
</type>
```

### events.xml Entries (for vehicles)
Creates spawn events:
```xml
<event name="ExpansionUAZ_Event">
    <nominal>2</nominal>
    <min>1</min>
    <max>3</max>
    <lifetime>3888000</lifetime>
    <children>
        <child type="ExpansionUAZ" max="3" min="1"/>
    </children>
</event>
```

## Backups

- Automatic backups created before any modification
- Stored in `./backups/` folder with timestamps
- Format: `types.xml.20240129_143022.bak`
- Keep these safe in case you need to revert!

## Finding Item Classnames

### Method 1: Check Mod Documentation
Most mods include a list of classnames in their Steam Workshop page or GitHub

### Method 2: DayZ Editor
1. Load the mod in DayZ Editor
2. Browse items in the editor
3. Copy classnames from item properties

### Method 3: Config Files
Look in the mod's `config.cpp` files (usually in the mod's `Addons` folder)

### Method 4: Community Resources
- [DayZ Expansion Wiki](https://github.com/salutesh/DayZ-Expansion-Scripts/wiki)
- Server admin Discord communities
- Mod-specific documentation

## Common Mods and Classnames

**DayZ-Expansion-Weapons:**
- ExpansionAK74, ExpansionAKS74U
- ExpansionM16A4, ExpansionM4A1
- ExpansionAWM, ExpansionMosin
- ExpansionDT11

**DayZ-Expansion-Vehicles:**
- ExpansionUAZ, ExpansionBus
- ExpansionTractor, ExpansionVodnik
- ExpansionGyrocopter, ExpansionMerlin

**Dabs Framework:**
- dbo_ prefixed items

**MMSZ:**
- MMSZ_Car, MMSZ_Bike, etc.

## Troubleshooting

**"File not found" error:**
- Check your paths in `mod_config.json`
- Use forward slashes (/) not backslashes (\)
- Make sure the XML files exist

**Items not spawning in-game:**
- Restart your server after XML changes
- Check that classnames are correct (case-sensitive!)
- Verify the mod is loaded on your server
- Check nominal/min values aren't too low

**Duplicate entries:**
- Script automatically skips duplicates
- If you see duplicates, your XML might have been manually edited with different casing

**XML structure broken:**
- Check your backups folder
- Restore from backup: copy `.bak` file over the original
- Run the script again

**No vehicle spawns:**
- Check `vehicle_events.enabled` is `true` in config
- Vehicles need both types.xml entry AND events.xml entry to spawn as world events
- Check event nominal/min/max values

## Advanced Tips

### Batch Processing Multiple Mods
Create separate JSON files for each mod:
```bash
python dayz_mod_xml_automator.py
# Select option 2
# Enter: expansion_weapons.json
# Run again for expansion_vehicles.json
```

### Custom Categories
Edit config to use different categories:
```json
"weapons": {
    "category": "custom_military_tier3"
}
```

### Location-Specific Spawns
Adjust usage tags for specific areas:
```json
"usage": ["Military", "MilitaryEast", "PolicePlus"]
```

Common usage tags:
- Military, Police, Hunting
- Industrial, Farm, Village, Town, City
- Medic, Firefighter, Prison

### Testing Values
Start conservative:
```json
"nominal": 3,
"min": 1
```
Then increase if too rare.

## File Structure

```
your-server/
├── dayz_mod_xml_automator.py
├── mod_config.json (auto-generated)
├── example_mod_items.json
├── backups/
│   ├── types.xml.20240129_120000.bak
│   └── cfgeventspawns.xml.20240129_120000.bak
└── mpmissions/
    └── dayzOffline.chernarusplus/
        ├── db/
        │   ├── types.xml
        │   └── spawnabletypes.xml
        └── cfgeventspawns.xml
```

## Safety Features

✅ Never overwrites without backup
✅ Checks for existing entries
✅ Validates XML structure
✅ Case-sensitive duplicate detection
✅ Preserves existing configurations

## Support

For issues or questions:
1. Check your `mod_config.json` paths
2. Verify classnames are correct
3. Check backups if something breaks
4. Review DayZ server logs for errors

## Future Enhancements

Planned features:
- [ ] Automatic config.cpp parsing
- [ ] spawnabletypes.xml support
- [ ] Trader config generation
- [ ] GUI interface
- [ ] Batch mod folder scanning
- [ ] Templates for popular mods

## License

Free to use and modify for your DayZ server!

---

**Happy Modding!** 🎮
