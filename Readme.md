# Smart Load Manager Package Setup Guide

This package provides dashboard sensors, scripts, and automations that work **alongside** your existing Smart Load Manager blueprint automation. It reuses your blueprint's input helpers and entity selections for seamless integration.

## Quick Setup

### Step 1: Enable Packages in Home Assistant
Add to your `configuration.yaml`:
```yaml
homeassistant:
  packages: !include_dir_named packages
```

### Step 2: Install Package
1. Create `/config/packages/` directory
2. Copy `/packages/smart_load_manager.yaml` to `/config/packages/`
3. Update entity names (see Entity Mapping below)
4. Restart Home Assistant

### Step 3: Verify
Check **Developer Tools** → **States** for new entities like:
- `sensor.grid_demand_calculated`
- `sensor.current_peak_limit`
- `sensor.power_margin`

## Entity Mapping from Blueprint

The package is designed to **reuse your existing blueprint input helpers** and entity selections. Here's how to configure it:

### Step 1: Extract Your Blueprint Configuration
1. **Settings** → **Automations & Scenes**
2. Find "Smart Load Manager" automation  
3. **Edit** → switch to **YAML mode** to see your configuration

### Step 2: Map Your Entities
From your blueprint YAML, you'll see something like this:

```yaml
use_blueprint:
  path: smart_load_manager_blueprint.yaml
  input:
    pv_power: sensor.solaredge_current_power
    grid_power: sensor.grid_power_import_export
    home_consumption: sensor.house_consumption_w
    battery_soc: sensor.solaredge_battery_level
    emergency_override: input_boolean.emergency_charging_mode
    tesla_charger: number.tesla_wall_connector_charging_amps
    hyundai_charger: number.ioniq5_charging_current
    heat_pump: switch.heat_pump_main
    sauna: switch.sauna_heater
    # ... etc
```

### Step 3: Update Package File
Replace the placeholder entities in `smart_load_manager.yaml` with your actual entities:

```yaml
# Power sensors:
sensor.home_consumption → sensor.house_consumption_w
sensor.pv_power → sensor.solaredge_current_power
sensor.grid_power → sensor.grid_power_import_export
sensor.battery_soc → sensor.solaredge_battery_level

# EV chargers:
number.tesla_charging_current → number.tesla_wall_connector_charging_amps
number.hyundai_charging_current → number.ioniq5_charging_current

# Emergency control:
input_boolean.emergency_override → input_boolean.emergency_charging_mode

# Optional loads (only if you configured them):
switch.heat_pump → switch.heat_pump_main
switch.sauna → switch.sauna_heater
# ... etc
```

### Step 4: Reuse Blueprint Input Helpers
The package will reference your existing blueprint input helpers for peak limits and battery settings. **No additional input helpers needed!**

The package templates will automatically use the same input helpers that your blueprint created.

## Package Features

### 📊 Key Sensors
- **Grid Demand**: `sensor.grid_demand_calculated`
- **Peak Limits**: `sensor.current_peak_limit` (dynamic based on time/conditions)
- **Power Margin**: `sensor.power_margin`
- **System Status**: `sensor.load_manager_status`
- **EV Power**: `sensor.tesla_charging_power`, `sensor.hyundai_charging_power`

### 🤖 Scripts & Automations
- **Reset Loads**: `script.smart_load_reset_all_loads`
- **Emergency Controls**: Activate/deactivate scripts
- **Notifications**: Automatic alerts for emergency mode and peak warnings
- **Logging**: State change tracking

### 📈 Statistics & Monitoring
- Daily peak and average demand tracking
- Weekend/summer detection
- Over-peak alerts

## Customization

### Modify Peak Limits
Use the existing input helpers from your blueprint automation:
- **Settings** → **Devices & Services** → **Helpers**
- Search for "peak_p1", "peak_p2", etc.
- Adjust values as needed

### Battery Thresholds
Similarly, modify your blueprint's battery thresholds:
- `input_number.battery_soc_min` - Minimum SoC for load suspension
- `input_number.battery_soc_emergency` - Emergency charging threshold

### Add More Devices
To add additional loads to the package:
1. Add switch entities to the appropriate sections
2. Update the suspension logic in the scripts
3. Add status tracking in the template sensors

### Customize Notifications
Modify the automation messages and conditions in the automation section of the package file.

## Troubleshooting

### Package Not Loading
- Check `configuration.yaml` has the packages include
- Verify file is in `/config/packages/` directory
- Check YAML syntax with Configuration Check

### Entities Missing
- Verify all referenced entities exist in your system
- Check Home Assistant logs for errors
- Use Developer Tools to test template syntax

### Values Not Updating
- Check that source sensors are working
- Verify template logic in Developer Tools → Template
- Restart Home Assistant if needed

## Maintenance

### Regular Updates
- Monitor daily statistics sensors
- Adjust peak limits based on usage patterns
- Review notification messages for relevance

### Backup
The entire Smart Load Manager can be backed up by saving the single package file.

### Removal
To remove all Smart Load Manager entities, simply delete the package file and restart Home Assistant.

This package approach gives you a complete, self-contained Smart Load Manager system that's easy to maintain and customize!
