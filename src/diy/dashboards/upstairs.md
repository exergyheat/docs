<!--explain in a detailed manner what this dashboard shows, how to download and configure it, and how to customize and expand upon it-->
# Upstairs Avalon Q + Furnace Hybrid Dashboard - Exergy HA Dashboard

A dashboard for monitoring and controlling a hybrid Avalon Q bitcoin heater integrated with a furnace backup system, featuring wall thermostat control and adjustable power modes.

## Overview

This dashboard provides:
- Venstar wall thermostat interface
- Three-tier power mode control (Super, Standard, Eco)
- Real-time temperature monitoring
- Mining statistics and earnings tracking
- Dual heating source coordination (Bitcoin heater + furnace)

## Before You Start

This dashboard requires:
- **Miner connected** - [Canaan Avalon Home Integration](../integrations/exergy-canaan.md)
- **Thermostat connected** - Venstar or other smart thermostat in HA
- **Ocean pool integration** - [Ocean Mining Pool Integration](../integrations/ocean-pool.md) for earnings data
<!--
## System Layout

```
┌─────────────────────────────────────────────────────────┐
│  Upstairs Zone - Avalon Q + Furnace                     │
├─────────────────────┬───────────────────────────────────┤
│                     │                                   │
│   ┌───────────┐     │   Power Modes:                    │
│   │           │     │   ┌───────┬────────┬──────┐      │
│   │   68°F    │     │   │ Super │Standard│ Eco  │      │
│   │   ───     │     │   └───────┴────────┴──────┘      │
│   │   65°F    │     │                                   │
│   │  target   │     │   ┌─────────────────────────┐    │
│   │           │     │   │ Mining Stats            │    │
│   └───────────┘     │   │ State: Mining           │    │
│                     │   │ Level: Standard         │    │
│  Wall Thermostat    │   │ Hashrate: 45 TH/s       │    │
│                     │   │ Power: 1,800W           │    │
│                     │   │ Lifetime: 450,000 sats  │    │
│                     │   └─────────────────────────┘    │
└─────────────────────┴───────────────────────────────────┘
```
-->

## Equipment

- **Primary Heat**: Avalon Q Bitcoin Heater
- **Backup Heat**: Furnace (HVAC system)
- **Control**: Venstar Wall Thermostat
- **Power Modes**: Super (~1.7kW), Standard (~1.2kW), Eco (~0.8kW)

## Required Integrations

- **Venstar**: Thermostat integration (Optional to use the Generic Thermostat instead)
- **Exergy Avalon Home Miner**: Miner Integration
- **OCEAN Mining Pool**: Exergy Integration to pull in OCEAN Mining Pool Stats

## Installation

### Add to Existing Dashboard or Create new

1. Copy the YAML below and add it as a new view to your existing dashboard or create a new one.
2. Go to **Settings → Dashboards**
3. Select an existing dashboard or create a new one
4. Click three dots → **Edit Dashboard** → **Add View**
5. Switch to YAML mode and paste the view configuration below
6. Update entity IDs to match your system
7. Save

## Dashboard YAML

### Complete Upstairs View

```yaml
- type: sections
  max_columns: 4
  title: Upstairs
  path: upstairs
  sections:
    - type: grid
      cards:
        - type: heading
          heading: Venstar Wall Thermostat Control
          heading_style: subtitle
        - type: thermostat
          entity: climate.upstairs_thermostat
          show_current_as_primary: false
          features:
            - style: icons
              type: climate-hvac-modes
              hvac_modes:
                - heat
                - 'off'
        - type: heading
          icon: ''
          heading: Mode Control
          heading_style: subtitle
        - type: horizontal-stack
          cards:
            - name: Super
              show_name: true
              show_icon: true
              type: button
              entity: sensor.avalon_q_hashrate
              icon: mdi:fire
              icon_height: 30px
              show_state: false
              hold_action:
                action: none
              tap_action:
                action: perform-action
                perform_action: select.select_option
                target:
                  entity_id: select.avalon_q_work_level
                data:
                  option: Super
            - name: Standard
              show_name: true
              show_icon: true
              type: button
              entity: sensor.avalon_q_hashrate
              icon: mdi:lightning-bolt
              icon_height: 30px
              show_state: false
              hold_action:
                action: none
              tap_action:
                action: perform-action
                perform_action: select.select_option
                target:
                  entity_id: select.avalon_q_work_level
                data:
                  option: Standard
            - name: Eco
              show_name: true
              show_icon: true
              type: button
              entity: sensor.avalon_q_hashrate
              icon: mdi:leaf
              icon_height: 30px
              show_state: false
              hold_action:
                action: none
              tap_action:
                action: perform-action
                perform_action: select.select_option
                target:
                  entity_id: select.avalon_q_work_level
                data:
                  option: Eco
    - type: grid
      cards:
        - type: heading
          heading: Stats
          heading_style: subtitle
        - graph: line
          type: sensor
          detail: 1
          icon: mdi:home-thermometer
          grid_options:
            columns: 12
            rows: 2
          name: Room Temperature
          entity: sensor.upstairs_thermostat_space_temperature
        - type: entities
          entities:
            - entity: sensor.avalon_q_state
              name: State
              icon: mdi:list-status
              secondary_info: last-updated
            - entity: sensor.avalon_q_work_level
              name: Heating Level
              icon: mdi:arrow-expand
              secondary_info: last-updated
            - entity: sensor.avalon_q_hashrate
              name: Hashrate
              secondary_info: last-updated
              icon: mdi:chart-timeline-variant
            - entity: sensor.avalon_q_power
              secondary_info: last-updated
            - entity: sensor.q_lifetime_earnings
              icon: mdi:bitcoin
              secondary_info: last-updated
              name: Lifetime Earnings
          footer:
            type: graph
            entity: sensor.q_lifetime_earnings
            detail: 1
            hours_to_show: 336
          show_header_toggle: false
          state_color: false
  header:
    card:
      type: markdown
      text_only: true
      content: |-
        **Hello {{ user }}**
        Welcome to your Hybrid Avalon Q + Furnace System
  badges:
    - type: entity
      show_name: true
      show_state: false
      show_icon: true
      name: Pool Dashboard
      tap_action:
        action: url
        url_path: https://ocean.xyz/stats/[YOUR OCEAN ADDRESS].[WORKER]
      icon: mdi:pickaxe
      entity: update.advanced_ssh_web_terminal_update #This can be anything, it's just a placeholder
    - type: entity
      show_name: true
      show_state: false
      show_icon: true
      entity: update.advanced_ssh_web_terminal_update #This can be anything, it's just a placeholder
      tap_action:
        action: url
        url_path: https://support.exergyheat.com
      icon: mdi:face-agent
      name: Support
    - type: entity
      show_name: true
      show_state: false
      show_icon: true
      entity: update.advanced_ssh_web_terminal_update #This can be anything, it's just a placeholder
      tap_action:
        action: url
        url_path: https://docs.exergyheat.com
      icon: mdi:book
      name: Documentation
```

## Entity ID Reference

Update these placeholders with your actual entities:

| Placeholder | Description | How to Find |
|-------------|-------------|-------------|
| **climate.upstairs_thermostat** | Venstar wall thermostat | Settings → Devices → Venstar Thermostat |
| **sensor.upstairs_thermostat_space_temperature** | Current room temperature | Same device as thermostat |
| **sensor.avalon_q_state** | Miner operational state | Settings → Devices → Avalon Q |
| **sensor.avalon_q_work_level** | Current power mode | Settings → Devices → Avalon Q |
| **select.avalon_q_work_level** | Power mode selector | Settings → Devices → Avalon Q |
| **sensor.avalon_q_hashrate** | Mining hashrate | Settings → Devices → Avalon Q |
| **sensor.avalon_q_power** | Power consumption | Settings → Devices → Avalon Q |
| **sensor.q_lifetime_earnings** | Bitcoin earnings | Settings → Integrations → OCEAN Mining Pool → Select Miner → Q → Lifetime Earnings |

## Power Mode Details

### Super Mode
- **Power**: ~1,700W
- **Hashrate**: ~90 TH/s
- **Use Case**: Maximum heat output, coldest days
- **Noise**: Loudest
- **Earnings**: Highest

### Standard Mode
- **Power**: ~1,200W
- **Hashrate**: ~75-80 TH/s
- **Use Case**: Normal heating needs
- **Noise**: Moderate
- **Earnings**: Medium

### Eco Mode
- **Power**: ~800W
- **Hashrate**: ~50 TH/s
- **Use Case**: Mild days, shoulder season
- **Noise**: Quietest
- **Earnings**: Lower

## Customization

### Change Power Mode Options

To add or modify power modes, update the button tap actions:

```yaml
tap_action:
  action: perform-action
  perform_action: select.select_option
  target:
    entity_id: select.avalon_q_work_level
  data:
    option: Custom  # Add your custom mode here
```

### Customize Temperature Graph

Adjust the temperature history graph display:

```yaml
- graph: line
  type: sensor
  detail: 1  # Change detail level (1-2)
  icon: mdi:home-thermometer
  grid_options:
    columns: 12  # Adjust width (1-12)
    rows: 2      # Adjust height
  name: Room Temperature
  entity: sensor.upstairs_thermostat_space_temperature
```

### Add Additional Temperature Sensors

Add more temperature monitoring to the stats section:

```yaml
- type: tile
  entity: sensor.bedroom_temperature
  name: Bedroom Temp
  vertical: false
  features_position: bottom
```

### Customize Pool Dashboard Link

Update the badge to point to your specific pool worker:

```yaml
- type: entity
  show_name: true
  show_state: false
  show_icon: true
  name: Pool Dashboard
  tap_action:
    action: url
    url_path: https://ocean.xyz/stats/YOUR_WALLET.YOUR_WORKER_NAME
  icon: mdi:pickaxe
  entity: update.advanced_ssh_web_terminal_update #This can be anything, it's just a placeholder
```

## Advanced: Temperature-Based Mode Automation

Automatically adjust power modes based on outdoor temperature:

```yaml
automation:
  - alias: "Upstairs - Auto Adjust Power Mode"
    trigger:
      - platform: numeric_state
        entity_id: sensor.outdoor_temperature
        below: 20
    action:
      - service: select.select_option
        target:
          entity_id: select.avalon_q_work_level
        data:
          option: Super

  - alias: "Upstairs - Eco Mode When Mild"
    trigger:
      - platform: numeric_state
        entity_id: sensor.outdoor_temperature
        above: 40
    action:
      - service: select.select_option
        target:
          entity_id: select.avalon_q_work_level
        data:
          option: Eco
```

## Individual Card Examples

### Standalone Thermostat Card

```yaml
type: thermostat
entity: climate.upstairs_thermostat
show_current_as_primary: false
features:
  - style: icons
    type: climate-hvac-modes
    hvac_modes:
      - heat
      - 'off'
```

### Power Mode Buttons Only

```yaml
type: horizontal-stack
cards:
  - name: Super
    type: button
    entity: sensor.avalon_q_hashrate
    icon: mdi:fire
    tap_action:
      action: perform-action
      perform_action: select.select_option
      target:
        entity_id: select.avalon_q_work_level
      data:
        option: Super
  - name: Standard
    type: button
    entity: sensor.avalon_q_hashrate
    icon: mdi:lightning-bolt
    tap_action:
      action: perform-action
      perform_action: select.select_option
      target:
        entity_id: select.avalon_q_work_level
      data:
        option: Standard
  - name: Eco
    type: button
    entity: sensor.avalon_q_hashrate
    icon: mdi:leaf
    tap_action:
      action: perform-action
      perform_action: select.select_option
      target:
        entity_id: select.avalon_q_work_level
      data:
        option: Eco
```

### Mining Statistics Card

```yaml
type: entities
title: Avalon Q Stats
entities:
  - entity: sensor.avalon_q_state
    name: State
    icon: mdi:list-status
  - entity: sensor.avalon_q_work_level
    name: Heating Level
  - entity: sensor.avalon_q_hashrate
    name: Hashrate
  - entity: sensor.avalon_q_power
    name: Power Draw
  - entity: sensor.q_lifetime_earnings
    name: Lifetime Earnings
    icon: mdi:bitcoin
footer:
  type: graph
  entity: sensor.q_lifetime_earnings
  detail: 1
  hours_to_show: 336
```

## Troubleshooting

### Thermostat Not Responding

- Verify Venstar integration is configured
- Check thermostat is online and connected to WiFi
- Confirm entity ID matches your actual thermostat

### Power Mode Buttons Not Working

- Ensure Avalon Q integration is installed
- Verify **select.avalon_q_work_level** entity exists
- Check miner is online and accessible

### Temperature Graph Not Displaying

- Confirm **sensor.upstairs_thermostat_space_temperature** exists
- Check sensor is reporting data
- Verify entity has a history (may take a few minutes after setup)

### Lifetime Earnings Showing 0

- Ensure Ocean pool integration is configured
- Verify template sensor is created in **configuration.yaml**
- Check **sensor.q_lifetime_earnings** exists and has data

### Badges Not Clickable

- Verify URLs in **tap_action** are correct
- Update pool worker name and wallet address
- Use a real entity (like **update.advanced_ssh_web_terminal_update**) or create a dummy input_text

## Mobile View

The dashboard automatically adapts to mobile screens:
- Cards stack vertically
- Power mode buttons remain side-by-side and scrollable
- Temperature graph scales to screen width

## Resources

- [HVAC Integrated Thermostat Control](../blueprints/hvac.md) - Automation for this setup
- [Home Assistant Sections Dashboard](https://www.home-assistant.io/dashboards/sections/)
- [Venstar Integration](https://www.home-assistant.io/integrations/venstar/)
- [Template Sensors](https://www.home-assistant.io/integrations/template/)
- [Ocean Mining Pool](https://ocean.xyz/)
- [Exergy GitHub - Dashboard Templates](https://github.com/exergyheat)
