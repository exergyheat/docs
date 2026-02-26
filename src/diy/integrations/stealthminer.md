<!--explain in a detailed manner what the stealthminer/luxos integration does, how to install it and use it-->
# StealthMiner / LuxOS - Exergy Home Assistant Integration

Monitor and control miners running LuxOS firmware directly in Home Assistant. Full control over power, profiles, and mining operations.

## Before You Start

Before installing this integration:

1. **Home Assistant 2024.1.0 or newer** - This integration requires HA 2024.1.0+.

2. **HACS installed** - This is a custom integration distributed via HACS. See our [HACS installation guide](../brains/install-hacs.md) if you need to install HACS.

3. **Miner running LuxOS firmware** - Your miner must be flashed with LuxOS firmware.

4. **Miner IP address and port 4028 accessible** - LuxOS API runs on port 4028.

## What is LuxOS?

[LuxOS](https://luxor.tech/firmware) is alternative firmware for bitcoin miners that provides:

- **Enhanced control** - Granular power and performance settings
- **ATM (Auto-Tune Mode)** - Automatic optimization for best performance
- **Multiple profiles** - Switch between preset configurations
- **API access** - Full control via HTTP API on port 4028

## Supported Hardware

LuxOS firmware runs on various miner models including Antminer S19 series and other compatible hardware. Check LuxOS compatibility documentation for your specific model.

## Integration Features

The Exergy StealthMiner/LuxOS integration provides comprehensive monitoring and control:

### Sensors

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Hashrate (5s) | `sensor.stealthminer_hashrate_5s` | 5-second hashrate average |
| Hashrate (1m) | `sensor.stealthminer_hashrate_1m` | 1-minute hashrate average |
| Hashrate (15m) | `sensor.stealthminer_hashrate_15m` | 15-minute hashrate average |
| Hashrate (30m) | `sensor.stealthminer_hashrate_30m` | 30-minute hashrate average |
| Hashrate (avg) | `sensor.stealthminer_hashrate_avg` | Overall average hashrate |
| Power | `sensor.stealthminer_power` | Current power consumption (W) |
| Efficiency | `sensor.stealthminer_efficiency` | Efficiency in W/TH |
| Board Temperature | `sensor.stealthminer_board_temp` | Hashboard temperature |
| Fan Speed (%) | `sensor.stealthminer_fan_percent` | Fan speed percentage |
| Fan Speed (RPM) | `sensor.stealthminer_fan_rpm` | Fan speed in RPM |
| Shares Accepted | `sensor.stealthminer_shares_accepted` | Total accepted shares |
| Shares Rejected | `sensor.stealthminer_shares_rejected` | Total rejected shares |
| Shares Stale | `sensor.stealthminer_shares_stale` | Total stale shares |
| Active Pool | `sensor.stealthminer_pool` | Current pool URL |
| Active Profile | `sensor.stealthminer_profile` | Active profile name |
| Status | `sensor.stealthminer_status` | Miner operational status |

### Binary Sensors

| Binary Sensor | Entity Example | Description |
|---------------|----------------|-------------|
| Connectivity | `binary_sensor.stealthminer_connectivity` | Connection to miner |
| Pool Connected | `binary_sensor.stealthminer_pool_connected` | Pool connection status |
| ATM Active | `binary_sensor.stealthminer_atm_active` | Auto-Tune Mode state |
| Mining Active | `binary_sensor.stealthminer_mining` | Mining activity status |

### Controls

| Control | Entity Example | Type | Description |
|---------|----------------|------|-------------|
| ATM Toggle | `switch.stealthminer_atm` | Switch | Enable/disable Auto-Tune Mode |
| Sleep Mode | `switch.stealthminer_sleep` | Switch | Put miner in sleep mode |
| Profile Selector | `select.stealthminer_profile` | Select | Choose operating profile |
| Power Limit | `number.stealthminer_power_limit` | Number | Set power consumption limit (W) |
| Reboot | `button.stealthminer_reboot` | Button | Reboot the miner |
| Factory Reset | `button.stealthminer_reset` | Button | Factory reset (caution!) |
| Wake | `button.stealthminer_wake` | Button | Wake from sleep mode |

> **Note:** Entity IDs are generated based on your miner name. Find actual IDs at **Settings > Devices & Services > StealthMiner > [your device]**.

## Installation

### Step 1: Install via HACS

1. Open Home Assistant
2. Navigate to **HACS > Integrations**
3. Click **+ Explore & Download Repositories**
4. Search for "StealthMiner" or "Exergy StealthMiner"
5. Click **Download**
6. Restart Home Assistant

### Step 2: Add Your Miner

1. Go to **Settings > Devices & Services**
2. Click **+ Add Integration**
3. Search for "StealthMiner"
4. Enter your miner's IP address
5. Click **Submit**

| Setting | Default | Description |
|---------|---------|-------------|
| **IP Address** | — | Your miner's local IP address |
| **Port** | 4028 | LuxOS API port |

### Step 3: Verify

The integration creates a device with all sensors and controls. Check the device page to see all available entities.

## Understanding LuxOS Features

### Auto-Tune Mode (ATM)

ATM automatically optimizes your miner for best performance:

- Monitors chip health and temperatures
- Adjusts frequency and voltage dynamically
- Balances hashrate vs efficiency
- Recommended for most users

Toggle ATM with `switch.stealthminer_atm`.

### Profiles

LuxOS supports multiple operating profiles:

| Profile Type | Use Case |
|--------------|----------|
| Default | Standard operation |
| Efficiency | Maximize W/TH efficiency |
| Performance | Maximum hashrate |
| Custom | User-defined settings |

Switch profiles via `select.stealthminer_profile`.

### Sleep Mode

Sleep mode stops mining while keeping the miner responsive:

- Fans spin down
- Power consumption drops to idle
- Quick wake-up compared to full power cycle
- Useful for demand response or time-of-use optimization

Control with `switch.stealthminer_sleep` and `button.stealthminer_wake`.

### Power Limiting

Set a maximum power draw with `number.stealthminer_power_limit`:

- Miner adjusts performance to stay under limit
- Useful for electrical capacity constraints
- Combine with time-of-use automations

## Dashboard Integration

Create a comprehensive miner control panel:

```yaml
type: vertical-stack
cards:
  - type: entities
    title: StealthMiner Status
    entities:
      - entity: sensor.stealthminer_hashrate_15m
        name: Hashrate (15m)
      - entity: sensor.stealthminer_power
        name: Power
      - entity: sensor.stealthminer_efficiency
        name: Efficiency
      - entity: sensor.stealthminer_board_temp
        name: Temperature
      - entity: binary_sensor.stealthminer_mining
        name: Mining Active

  - type: entities
    title: Controls
    entities:
      - entity: switch.stealthminer_atm
        name: Auto-Tune Mode
      - entity: switch.stealthminer_sleep
        name: Sleep Mode
      - entity: select.stealthminer_profile
        name: Profile
      - entity: number.stealthminer_power_limit
        name: Power Limit
```

## Automation Ideas

### Thermostat Integration

Use your miner as a heater controlled by room temperature:

```yaml
automation:
  - alias: "StealthMiner thermostat - wake on cold"
    trigger:
      - platform: numeric_state
        entity_id: sensor.room_temperature
        below: 68
    condition:
      - condition: state
        entity_id: switch.stealthminer_sleep
        state: "on"
    action:
      - service: button.press
        target:
          entity_id: button.stealthminer_wake

  - alias: "StealthMiner thermostat - sleep on warm"
    trigger:
      - platform: numeric_state
        entity_id: sensor.room_temperature
        above: 72
    condition:
      - condition: state
        entity_id: switch.stealthminer_sleep
        state: "off"
    action:
      - service: switch.turn_on
        target:
          entity_id: switch.stealthminer_sleep
```

### Time-of-Use Power Limiting

Reduce power during peak electricity rates:

```yaml
automation:
  - alias: "StealthMiner peak rate power limit"
    trigger:
      - platform: time
        at: "16:00:00"
    action:
      - service: number.set_value
        target:
          entity_id: number.stealthminer_power_limit
        data:
          value: 2000

  - alias: "StealthMiner off-peak full power"
    trigger:
      - platform: time
        at: "21:00:00"
    action:
      - service: number.set_value
        target:
          entity_id: number.stealthminer_power_limit
        data:
          value: 3500
```

### Pool Disconnect Alert

```yaml
automation:
  - alias: "StealthMiner pool disconnect alert"
    trigger:
      - platform: state
        entity_id: binary_sensor.stealthminer_pool_connected
        to: "off"
        for:
          minutes: 5
    action:
      - service: notify.mobile_app
        data:
          message: "StealthMiner lost pool connection"
```

### Profile Switching Based on Conditions

```yaml
automation:
  - alias: "StealthMiner efficiency mode during peak"
    trigger:
      - platform: time
        at: "14:00:00"
    action:
      - service: select.select_option
        target:
          entity_id: select.stealthminer_profile
        data:
          option: "Efficiency"

  - alias: "StealthMiner performance mode off-peak"
    trigger:
      - platform: time
        at: "22:00:00"
    action:
      - service: select.select_option
        target:
          entity_id: select.stealthminer_profile
        data:
          option: "Performance"
```

## Troubleshooting

### Integration not finding miner

1. Verify miner is powered and connected to network
2. Confirm IP address is correct
3. Check port 4028 is accessible: `telnet [IP] 4028`
4. Verify LuxOS firmware is installed and running

### Controls not responding

1. Some controls require specific LuxOS versions
2. Check miner is not in a transitional state
3. Reload the integration
4. Check Home Assistant logs for API errors

### ATM not working as expected

1. ATM requires time to tune (hours to days for optimal results)
2. Check chip health in LuxOS web interface
3. Manual profile changes may override ATM settings

### Entities showing "Unavailable"

1. Check miner is online and reachable
2. Verify port 4028 connectivity
3. Reload the integration: **Settings > Devices & Services > StealthMiner > ⋮ > Reload**
4. Review Home Assistant logs

## What's Next?

### Monitor Pool Stats
- [Ocean Pool Integration](./ocean-pool.md) - For Ocean pool stats
- [Public Pool Integration](./public-pool.md) - For self-hosted pools

### Set Up Heating Automation
- [Space Heater Thermostat Control](../blueprints/space-heater.md) - Temperature control
- [HVAC Integration](../blueprints/hvac.md) - Whole-home integration
- [Time-of-Use Control](../blueprints/time-of-use.md) - Optimize around electricity rates

### Build a Dashboard
- [Dashboard Templates](../dashboards/overview.md) - Pre-built interfaces

## Source Code & Support

- **GitHub:** [github.com/exergyheat/ha-integration-stealthminer](https://github.com/exergyheat/ha-integration-stealthminer)
- **Support Forum:** [support.exergyheat.com](https://support.exergyheat.com)
