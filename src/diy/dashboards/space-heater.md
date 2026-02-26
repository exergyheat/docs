<!--explain in a detailed manner what this dashboard shows, how to download and configure it, and how to customize and expand upon it-->
# Space Heater Digital Thermostat - Exergy HA Dashboard

A dashboard that displays your miner as a digital thermostat, showing temperature, controls, and mining stats in one view.

## Preview

The dashboard provides:
- Large temperature display (current and target)
- On/Off control
- Work mode and level selectors
- Mining stats (hashrate, power, earnings)
- Temperature graph over time

## Installation

### Method 1: Import Full Dashboard

1. Download the YAML from [GitHub](https://github.com/exergyheat)
2. Go to **Settings → Dashboards**
3. Click **Add Dashboard** → Give it a name
4. Enter the dashboard → Click three dots → **Raw configuration editor**
5. Paste the YAML
6. Update entity IDs to match your system
7. Save

### Method 2: Add Cards Individually

Add each card to an existing dashboard using the examples below.

## Dashboard YAML

### Full Dashboard Template

```yaml
title: Space Heater
views:
  - title: Thermostat
    path: thermostat
    cards:
      # Main Thermostat Card
      - type: thermostat
        entity: climate.miner_thermostat  # If using climate entity
        # Or use custom card below if using switch + sensor

      # Temperature Display
      - type: sensor
        entity: sensor.living_room_temperature
        name: Room Temperature
        graph: line
        hours_to_show: 24
        detail: 2

      # Miner Status Card
      - type: entities
        title: Miner Status
        entities:
          - entity: switch.avalon_mini_3_power
            name: Power
          - entity: select.avalon_mini_3_work_mode
            name: Mode
          - entity: select.avalon_mini_3_work_level
            name: Level
          - entity: sensor.avalon_mini_3_hashrate
            name: Hashrate
          - entity: sensor.avalon_mini_3_power
            name: Power Draw

      # Temperature Gauges
      - type: horizontal-stack
        cards:
          - type: gauge
            entity: sensor.avalon_mini_3_temperature_ambient
            name: Intake
            min: 32
            max: 120
            severity:
              green: 32
              yellow: 90
              red: 100
          - type: gauge
            entity: sensor.avalon_mini_3_temperature_output
            name: Exhaust
            min: 32
            max: 150
            severity:
              green: 32
              yellow: 120
              red: 140

      # Mining Stats
      - type: entities
        title: Mining Stats
        entities:
          - entity: sensor.ocean_hashrate_24h
            name: Pool Hashrate (24h)
          - entity: sensor.ocean_estimated_daily
            name: Est. Daily Sats
          - entity: sensor.ocean_unpaid_balance
            name: Unpaid Balance
```

## Individual Card Examples

### Thermostat-Style Temperature Card

Using a button-card (install via HACS):

```yaml
type: custom:button-card
entity: sensor.living_room_temperature
name: Living Room
show_state: true
styles:
  card:
    - height: 200px
  state:
    - font-size: 48px
    - font-weight: bold
  name:
    - font-size: 16px
```

### Simple On/Off Button

```yaml
type: button
entity: switch.avalon_mini_3_power
name: Miner Power
show_state: true
tap_action:
  action: toggle
icon: mdi:radiator
```

### Work Mode Selector

```yaml
type: entities
entities:
  - entity: select.avalon_mini_3_work_mode
    name: Operating Mode
  - entity: select.avalon_mini_3_work_level
    name: Power Level
```

### Temperature History Graph

```yaml
type: history-graph
title: Temperature History
hours_to_show: 24
entities:
  - entity: sensor.living_room_temperature
    name: Room
  - entity: sensor.avalon_mini_3_temperature_output
    name: Miner Exhaust
```

### Hashrate and Power Graph

```yaml
type: history-graph
title: Mining Performance
hours_to_show: 24
entities:
  - entity: sensor.avalon_mini_3_hashrate
    name: Hashrate
  - entity: sensor.avalon_mini_3_power
    name: Power
```

## Customization

### Entity ID Updates

Replace these placeholder entities with your actual entity IDs:

| Placeholder | Find Yours |
|-------------|------------|
| `switch.avalon_mini_3_power` | Settings → Devices → [Your Miner] |
| `sensor.living_room_temperature` | Settings → Devices → [Your Temp Sensor] |
| `sensor.ocean_*` | Settings → Devices → Ocean Pool |

### Theme Customization

Match your Home Assistant theme:

```yaml
type: entities
title: Miner Control
card_mod:
  style: |
    ha-card {
      background-color: var(--card-background-color);
      border-radius: 12px;
    }
```

### Mobile-Friendly Layout

Use `grid` layout for responsive design:

```yaml
type: grid
columns: 2
square: false
cards:
  - type: sensor
    entity: sensor.living_room_temperature
  - type: button
    entity: switch.avalon_mini_3_power
  # ... more cards
```

## Recommended Custom Cards

Install via HACS → Frontend:

| Card | Use |
|------|-----|
| **mini-graph-card** | Compact, beautiful graphs |
| **mushroom** | Modern, clean card designs |
| **button-card** | Advanced button customization |
| **thermostat-card** | Traditional thermostat look |

### Mini Graph Card Example

```yaml
type: custom:mini-graph-card
entities:
  - entity: sensor.living_room_temperature
    name: Temperature
  - entity: sensor.avalon_mini_3_temperature_output
    name: Exhaust
    y_axis: secondary
hours_to_show: 24
line_width: 2
hour24: true
show:
  labels: true
  points: false
```

## Mobile Dashboard

Create a separate mobile-optimized view:

```yaml
views:
  - title: Mobile
    path: mobile
    panel: false
    cards:
      - type: vertical-stack
        cards:
          - type: sensor
            entity: sensor.living_room_temperature
            name: Room Temp
          - type: button
            entity: switch.avalon_mini_3_power
            name: Miner
            show_state: true
            tap_action:
              action: toggle
          - type: entities
            entities:
              - sensor.avalon_mini_3_hashrate
              - sensor.ocean_estimated_daily
```

## Troubleshooting

### Cards not displaying

- Verify entity IDs are correct
- Check that custom cards are installed (HACS)
- Clear browser cache

### Data not updating

- Check integration is connected
- Verify miner is online
- Review Home Assistant logs

## Resources

- [Home Assistant Dashboard Docs](https://www.home-assistant.io/dashboards/)
- [HACS Frontend Cards](https://hacs.xyz/)
- [Exergy Docs on GitHub](https://github.com/exergyheat/docs)
