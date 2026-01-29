# Quick Start Guide - 5 Minutes to Automation!

## Step 1: Setup (First Time Only)

1. **Download the script** to your DayZ server folder
2. **Run it once** to generate config:
   ```bash
   python dayz_mod_xml_automator.py
   ```
3. **Edit `mod_config.json`** - Change these paths to match YOUR server:
   ```json
   "xml_paths": {
       "types": "./mpmissions/YOUR_MISSION_NAME/db/types.xml",
       "events": "./mpmissions/YOUR_MISSION_NAME/cfgeventspawns.xml"
   }
   ```

## Step 2: Add Your First Mod

### Easy Way (Interactive):
```bash
python dayz_mod_xml_automator.py
# Select: 1 (Interactive)
# Select: 1 (Weapons)
# Type each weapon classname and press Enter
# Press Enter on empty line when done
# Type: y (to confirm)
```

### Fast Way (JSON file):
1. Create `my_mod.json`:
   ```json
   {
       "weapons": ["ExpansionAK74", "ExpansionM16A4"],
       "vehicles": ["ExpansionUAZ"]
   }
   ```
2. Run:
   ```bash
   python dayz_mod_xml_automator.py
   # Select: 2 (JSON file)
   # Enter: my_mod.json
   ```

## Step 3: Restart Server

That's it! Your items are now in the loot tables.

## Common First-Time Questions

**Q: Where do I find classnames?**
A: Check the mod's Steam Workshop page, or use DayZ Editor to browse the mod's items.

**Q: Do I need to run this every time I add a mod?**
A: Yes, but it takes 30 seconds. Just enter the new classnames.

**Q: What if I break something?**
A: Check the `backups/` folder - automatic backups are created before every change!

**Q: Can I change spawn rates?**
A: Yes! Edit `mod_config.json` → `default_values` → adjust `nominal` and `min` values.

**Q: Vehicles not spawning?**
A: Vehicles need to be in BOTH types.xml and events.xml. This script does both automatically!

## Example: Adding DayZ-Expansion-Weapons

1. Find classnames (from mod documentation):
   - ExpansionAK74
   - ExpansionAKS74U  
   - ExpansionAWM
   - ExpansionM16A4

2. Run script, select Interactive mode

3. Choose "Weapons", paste each classname

4. Confirm - Done!

## Typical Workflow

```
1. Install new mod on server
2. Find classnames (Steam/Discord/Editor)
3. Run: python dayz_mod_xml_automator.py
4. Add classnames (interactive or JSON)
5. Restart server
6. Test in-game
7. Adjust spawn rates in config if needed
8. Run script again to update
```

## Pro Tips

💡 Keep a text file with all your mod classnames for easy reference
💡 Start with low spawn rates (nominal: 3-5) and increase if too rare  
💡 Use JSON files for mods with 10+ items
💡 The script won't add duplicates, so it's safe to run multiple times
💡 Check backups folder occasionally and clean up old backups

## Need Help?

See the full README.md for detailed configuration options!
