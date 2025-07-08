# Smart Load Manager Package Setup Guide

## Package Installation

### Step 1: Enable Packages in Home Assistant

Add this to your main `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

### Step 2: Create Package Directory

Create a `packages` directory in your Home Assistant config folder:
```
/config/
├── configuration.yaml
├── packages/
│   └── smart_load_manager.yaml
```

### Step 3: Install the Package

1. Copy `smart_load_manager.yaml` to `/config/packages/`
2. Update entity names to match your actual devices (see Entity Mapping below)
3. Restart Home Assistant

### Step 4: Verify Installation

After restart, check **Developer Tools** → **States** for new entities:
- `sensor.grid_demand_calculated`
- `sensor.current_peak_limit`
- `sensor.power_margin`
- `input_number.smart_load_peak_p1` through `p5`
- `input_boolean.smart_load_emergency_override`
- And many more...

## Entity Mapping

Replace these placeholder entities with your actual Home Assistant entities:

### Power Sensors (Required)
```yaml
# Change these in the package file:
sensor.home_consumption      → your_home_consumption_sensor
sensor.pv_power             → your_pv_power_sensor
sensor.battery_soc          → your_battery_soc_sensor
```

### EV Chargers (Required)
```yaml
number.tesla_charging_current   → your_tesla_charger_entity
number.hyundai_charging_current → your_hyundai_charger_entity
```

### Managed Loads (Optional - only if you have them)
```yaml
switch.heat_pump    → your_heat_pump_switch
switch.sauna        → your_sauna_switch
switch.ac_unit_1    → your_ac_unit_1_switch
switch.ac_unit_2    → your_ac_unit_2_switch
switch.ac_unit_3    → your_ac_unit_3_switch
switch.ac_unit_4    → your_ac_unit_4_switch
```

## Package Features

### 📊 Template Sensors
- **Grid Demand Calculator**: `sensor.grid_demand_calculated`
- **Dynamic Peak Limits**: `sensor.current_peak_limit`
- **Power Margin**: `sensor.power_margin`
- **Priority Level**: `sensor.current_priority_level`
- **EV Power Estimates**: `sensor.tesla_charging_power`, `sensor.hyundai_charging_power`
- **System Status**: `sensor.load_manager_status`
- **Statistics**: Daily peak and average demand

### 🎛️ Input Helpers
- **Peak Limits**: `input_number.smart_load_peak_p1` through `p5`
- **Battery Thresholds**: `input_number.smart_load_battery_soc_min/emergency`
- **Emergency Override**: `input_boolean.smart_load_emergency_override`
- **Info Display**: `input_text.smart_load_suspension_order`

### 🤖 Scripts
- **Reset All Loads**: `script.smart_load_reset_all_loads`
- **Emergency Activate**: `script.smart_load_emergency_activate`
- **Emergency Deactivate**: `script.smart_load_emergency_deactivate`

### 🔔 Automations
- **State Logger**: Logs all important changes
- **Emergency Notifications**: Alerts for emergency mode changes
- **Peak Warnings**: Notifications when exceeding limits

### 📈 Binary Sensors
- **Weekend Detection**: `binary_sensor.is_weekend`
- **Summer Detection**: `binary_sensor.is_summer`
- **Over Peak Alert**: `binary_sensor.over_peak_limit`

## Package Benefits

### ✅ **Complete Isolation**
- All Smart Load Manager entities in one file
- No conflicts with existing configuration
- Easy to backup, share, or remove

### ✅ **Organized Naming**
- All entities prefixed with `smart_load_` 
- Clear hierarchy and grouping
- No naming conflicts

### ✅ **Self-Contained**
- Everything needed for the dashboard
- All supporting automations included
- Ready-to-use scripts

### ✅ **Enhanced Features**
- Statistics sensors for daily tracking
- Notification system for alerts
- Multiple script options
- Detailed entity attributes

## Dashboard Integration

The package creates all entities needed for the dashboard. Update your dashboard YAML to use the new entity names:

```yaml
# Old entity names → New package entity names
sensor.grid_demand_calculated → sensor.grid_demand_calculated (same)
input_number.peak_p1 → input_number.smart_load_peak_p1
input_boolean.emergency_override → input_boolean.smart_load_emergency_override
# etc.
```

## Customization

### Modify Peak Limits
Edit the `initial:` values in the `input_number` section:
```yaml
smart_load_peak_p1:
  initial: 3.0  # Change this value
```

### Add More Devices
Add additional switches or sensors by copying the existing patterns.

### Customize Notifications
Modify the automation messages and conditions in the automation section.

### Change Time Schedules
Update the time-based priority logic in the template sensors.

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
