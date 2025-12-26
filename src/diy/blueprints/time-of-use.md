<!--explain in a detailed manner what this automation does, how to download and configure it, and how to customize and expand upon it-->
# Time of Use Control - Exergy HA Blueprint

Schedule your bitcoin miner to run during specific times based on electricity rates, daily schedules, or personal preferences.

## Before You Start

- **Miner connected to Home Assistant** - Install the [Canaan Avalon Home Integration](../integrations/exergy-canaan.md) first
- **Home Assistant with HACS** - See our [system configuration guide](../brains/rpi-ha-config.md) if needed
- **Know your electricity rate schedule** - Check your utility bill for TOU periods

## How It Works

This automation controls when your miner operates:

- **Run during cheap electricity** (off-peak hours)
- **Reduce or stop during expensive hours** (peak rates)
- **Follow a weekly schedule** for consistent operation
- **Combine with other automations** for advanced control

## Use Cases

### Time-of-Use Electricity Rates

Many utilities charge different rates throughout the day:
- **Off-peak**: Evenings, nights, weekends (cheapest)
- **Mid-peak**: Morning, late evening
- **On-peak**: Afternoon hours (most expensive)

Run your miner during off-peak hours to maximize profitability.

### Noise Considerations

- Run at full power when away from home
- Reduce to eco/night mode during sleeping hours
- Pause during important calls or meetings

### Seasonal Adjustments

- Run more during heating season
- Reduce during summer months
- Adjust based on weather conditions

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
| Miner Entity | Your miner's power switch |
| Schedule | Time periods and desired actions |

### Schedule Options

For each time period, choose:
- **Full Power**: Mining mode at maximum
- **Eco Mode**: Reduced power/noise
- **Night Mode**: Quiet operation
- **Off**: Completely powered down

## Example Schedules

### Basic Peak/Off-Peak (California TOU)

| Time | Action | Reason |
|------|--------|--------|
| 12:00 AM - 4:00 PM | Full Power | Off-peak rates |
| 4:00 PM - 9:00 PM | Eco Mode | Peak rates |
| 9:00 PM - 12:00 AM | Full Power | Off-peak rates |

### Work from Home

| Time | Day | Action | Reason |
|------|-----|--------|--------|
| 6:00 AM - 8:00 AM | Weekdays | Night Mode | Morning routine |
| 8:00 AM - 6:00 PM | Weekdays | Eco Mode | Working hours (noise) |
| 6:00 PM - 10:00 PM | Weekdays | Full Power | Evening heating |
| 10:00 PM - 6:00 AM | All days | Night Mode | Sleeping hours |
| All day | Weekends | Full Power | Away or flexible |

### Heating Season Focus

| Month | Default Mode |
|-------|--------------|
| Oct - Mar | Full Power (heating needed) |
| Apr - May | Eco Mode (mild weather) |
| Jun - Sep | Off or Eco (no heat needed) |

## Manual YAML Examples

### Simple Peak/Off-Peak

```yaml
automation:
  - alias: "Miner Off-Peak - Full Power"
    trigger:
      - platform: time
        at: "21:00:00"
    action:
      - service: switch.turn_on
        target:
          entity_id: switch.avalon_mini_3_power
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_mode
        data:
          option: "Mining"
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_level
        data:
          option: "Super"

  - alias: "Miner Peak - Eco Mode"
    trigger:
      - platform: time
        at: "16:00:00"
    action:
      - service: switch.turn_on
        target:
          entity_id: switch.avalon_mini_3_power
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_mode
        data:
          option: "Heating"
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_level
        data:
          option: "Eco"
```

### Weekly Schedule with Conditions

```yaml
automation:
  - alias: "Miner Weekday Work Hours - Quiet"
    trigger:
      - platform: time
        at: "08:00:00"
    condition:
      - condition: time
        weekday:
          - mon
          - tue
          - wed
          - thu
          - fri
    action:
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_mode
        data:
          option: "Night"

  - alias: "Miner Weekend - Full Power"
    trigger:
      - platform: time
        at: "00:00:00"
    condition:
      - condition: time
        weekday:
          - sat
          - sun
    action:
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_mode
        data:
          option: "Heating"
      - service: select.select_option
        target:
          entity_id: select.avalon_mini_3_work_level
        data:
          option: "Super"
```

### Dynamic Rate Integration

If you have a utility rate sensor (via integration):

```yaml
automation:
  - alias: "Miner Rate-Based Control"
    trigger:
      - platform: state
        entity_id: sensor.utility_rate_tier
    action:
      - choose:
          - conditions:
              - condition: state
                entity_id: sensor.utility_rate_tier
                state: "off_peak"
            sequence:
              - service: switch.turn_on
                target:
                  entity_id: switch.avalon_mini_3_power
          - conditions:
              - condition: state
                entity_id: sensor.utility_rate_tier
                state: "on_peak"
            sequence:
              - service: switch.turn_off
                target:
                  entity_id: switch.avalon_mini_3_power
```
**Note**: You can also make your own utility rate sensor via a helper entity

## Combining with Temperature Control

Time-of-use works alongside thermostat control:

1. Time-of-use sets the **maximum** allowed operation
2. Thermostat control decides **within** those windows

Example:
- Off-peak: Thermostat can control miner freely
- Peak: Miner stays off regardless of temperature

```yaml
automation:
  - alias: "Peak Hours - Override Thermostat"
    trigger:
      - platform: time
        at: "16:00:00"
    action:
      - service: climate.set_hvac_mode
        target:
          entity_id: climate.your_thermostat
        data:
          hvac_mode: "off"

  - alias: "Off-Peak - Enable Thermostat"
    trigger:
      - platform: time
        at: "21:00:00"
    action:
      - service: climate.set_hvac_mode
        target:
          entity_id: climate.your_thermostat
        data:
          hvac_mode: "heat"
```

## Tips

### Find Your Utility Rates

- Check your utility bill for TOU schedule
- Look for "Time of Use" or "TOU" rate plans
- Some utilities offer APIs or integrations

### Buffer Times

Add 15-30 minutes buffer before peak times:
- Prevents running into peak rates
- Allows miner to cool down gracefully

### Seasonal Adjustments

Create separate automations for:
- Winter (heating priority)
- Summer (mining only when profitable)
- Shoulder seasons (flexible)

### Dynamic Control of TOU Times

Create separate 'helper' entities to edit on the fly such as:
- Create a  date and/or time helper to set the different TOU periods
  - Easier to edit this entity than the automation
- Create a schedule helper to build out different triggers based on time periods

## Troubleshooting

### Miner not following schedule

- Check automation triggers are correct
- Verify time zone settings in HA
- Look for conflicting automations

### Overlapping automations

- Review all miner-related automations
- Use automation groups or modes
- Consider a single "scheduler" automation

## Resources

- [Home Assistant Time Trigger Docs](https://www.home-assistant.io/docs/automation/trigger/#time-trigger)
- [Exergy GitHub - Blueprints](https://github.com/exergyheat)
