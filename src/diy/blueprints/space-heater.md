<!--explain in a detailed manner what this automation does, how to download and configure it, and how to customize and expand upon it-->
# Space Heater Thermostat Control - Exergy HA Blueprint

Control your bitcoin miner like a traditional space heater with thermostatic temperature control.

## Before You Start

- **Miner connected to Home Assistant** - Install the [Canaan Avalon Home Integration](../integrations/exergy-canaan.md) first
- **Temperature sensor** - Zigbee, WiFi, or any HA-compatible sensor in the room
- **Home Assistant with HACS** - See our [system configuration guide](../brains/rpi-ha-config.md) if needed

## How It Works

This automation turns your miner on and off based on room temperature:

1. Temperature drops below target → Miner turns ON
2. Temperature rises above target → Miner turns OFF
3. Repeat to maintain desired temperature

Just like a space heater with a thermostat, but earning bitcoin while heating.

<!--## Blueprint Installation

### Method 1: Import from GitHub

1. Go to **Settings → Automations & Scenes → Blueprints**
2. Click **Import Blueprint**
3. Paste the URL:
   ```
   https://github.com/exergyheat/ha-blueprints/blob/main/time-of-use-control.yaml
   ```
4. Click **Preview Blueprint**
5. Click **Import Blueprint**

### Method 2: Manual YAML

1. Download from [GitHub](https://github.com/exergyheat)
2. Place in 
```
/config/blueprints/automation/exergy/time-of-use-control.yaml
```
3. Restart Home Assistant

**Note**: You can reach this directory either via SSH or through the [Home Assistant File Editor Add-on](https://github.com/home-assistant/addons/tree/master/configurator)
-->
## Automation Installation

To use the YAML automation examples in this guide:

1. Copy the desired YAML code from the examples below
2. In Home Assistant, navigate to **Settings → Automations & Scenes**
3. Click the **three-dot menu** (⋮) in the top right corner
4. Select **Edit in YAML**
5. Paste the copied YAML code at the bottom of your automations file
6. Adjust the **entity_id** values to match your specific miner entities
7. Click **Save**
8. The new automation(s) will appear in your automations list

Alternatively, you can create each automation through the UI and manually configure the triggers, conditions, and actions based on the examples.

## Configuration

### Required Inputs

| Input | Description |
|-------|-------------|
| Miner Entity | Your miner's power switch (e.g., **switch.avalon_mini_3_power**) |
| Temperature Sensor | Room temperature sensor (e.g., **sensor.living_room_temperature**) |
| Target Temperature | Desired room temperature |

**Note**: This example uses the Avalon Mini 3 onboard input or ambient temperature sensors for reporting to the thermostat. You could instead use a different temperature sensor in the room such as a Zigbee Temperature sensor.

### Optional Inputs

| Input | Default | Description |
|-------|---------|-------------|
| Hysteresis | 1.0°F | Temperature band to prevent rapid cycling |
| Minimum On Time | 5 min | Prevent short cycles |
| Minimum Off Time | 5 min | Allow miner to cool down |

## Understanding Hysteresis

Hysteresis prevents the miner from rapidly turning on and off:

**Example with 70°F target temperature and 1°F hysteresis:**
- Miner turns ON when temp drops to 69°F
  - This is the Cold Tolerance reference below
- Miner turns OFF when temp rises to 71°F
  - This is the Hot Tolerance reference below
- 2°F total swing (target ± hysteresis)

Increase hysteresis for longer, less frequent cycles. Decrease for tighter temperature control.

## Creating a Climate Helper

The recommended approach is to create a **Generic Thermostat** climate entity that controls your miner. This gives you a native thermostat interface in Home Assistant.

### Method 1: GUI Configuration

1. Navigate to **Settings → Devices & Services → Helpers**
2. Click **+ Create Helper**
3. Select **Generic Thermostat**
4. Configure the following:

| Setting | Value | Description |
|---------|-------|-------------|
| Name | Exergy Office | Name for your thermostat |
| Heater | switch.exergy_office_mini_3_power | Your miner's power switch |
| Target Sensor | sensor.exergy_office_sensor_temperature | Room temperature sensor |
| Min Temp | 60 | Minimum temperature setting |
| Max Temp | 80 | Maximum temperature setting |
| Target Temp | 70 | Default target temperature (this is editable from the dashboard after being deployed) |
| Cold Tolerance | 1 | How far below target before turning on |
| Hot Tolerance | 1 | How far above target before turning off |
| Min Cycle Duration | 10 seconds | Minimum time heater stays in one state |

5. Click **Create**

### Method 2: YAML Configuration

Add this to your **configuration.yaml** file:

```yaml
climate:
  - platform: generic_thermostat
    name: Exergy Office
    heater: switch.exergy_office_mini_3_power #Your Miner Power Switch
    target_sensor: sensor.exergy_office_mini_3_ambient_temperature #Or use an external sensor like: sensor.exergy_office_sensor_temperature
    min_temp: 60 #Set the lowest temp you'll allow this thermostat to be set
    max_temp: 80 #Set the highest temp you'll allow this thermostat to be set 
    ac_mode: false
    target_temp: 70 #Default starting temp
    cold_tolerance: 1
    hot_tolerance: 1
    min_cycle_duration:
      seconds: 10
    keep_alive:
      minutes: 3
    initial_hvac_mode: "heat" #Thermostat defaults to heating mode, can also be set to off
    precision: 0.5 #Amout you want to be able to set the thermostat. 0.5 = 70,70.5,80, etc.
```

After adding the YAML configuration:
1. Go to **Developer Tools → YAML**
2. Click **Check Configuration**
3. If valid, click **Restart** under **Server Controls**

### Understanding the Parameters

| Parameter | Description |
|-----------|-------------|
| **heater** | The switch entity that controls your miner |
| **target_sensor** | Temperature sensor that measures room temperature |
| **cold_tolerance** | Temperature drops this much below target before turning ON |
| **hot_tolerance** | Temperature rises this much above target before turning OFF |
| **min_cycle_duration** | Prevents rapid on/off cycling |
| **keep_alive** | Sends periodic updates to the heater switch |
| **initial_hvac_mode** | Mode when Home Assistant starts ("heat" or "off") |
| **precision** | Temperature adjustment increment (0.5 or 1.0) |

### Example: 70°F Target with 1° Tolerance
- Miner turns **ON** when temperature drops to **69°F** (70 - 1)
- Miner turns **OFF** when temperature rises to **71°F** (70 + 1)
- Total swing: 2°F

## Tips

### External Sensor Placement

- Place temperature sensor at "living height" (3-5 feet)
- Keep away from direct miner exhaust
- Avoid windows, exterior walls, or direct sunlight

### Multiple Rooms

- Create separate automations for each room/miner pair
- Each automation tracks its own temperature sensor

### Work Mode Considerations

- **Heating Mode**: Full power, maximum heat output
- **Eco Mode**: Reduced power, may not reach target temperature
- **Night Mode**: Quieter but less heat

Consider automations that adjust work mode based on time of day.

## Troubleshooting

### Miner cycling too frequently

- Increase hysteresis value
- Increase minimum on/off times
- Check sensor placement (may be too close to miner)

### Room never reaches target temperature

- Miner may not have enough heat output for space
- Check for drafts or heat loss
- Consider running in higher power mode

### Temperature overshoots

- Miner continues heating during shutdown
- Increase hysteresis slightly
- Normal behavior - mining hardware retains heat

## Dashboard Integration

See [Space Heater Digital Thermostat](../dashboards/space-heater.md) for a matching dashboard to control this automation visually.

## Resources

- [Home Assistant Automation Docs](https://www.home-assistant.io/docs/automation/)
- [Generic Thermostat Integration](https://www.home-assistant.io/integrations/generic_thermostat/)
- [Exergy GitHub](https://github.com/exergyheat)
