<!--explain in a detailed manner what the bitaxe integration does, how to install it and use it-->
# Bitaxe - Exergy Home Assistant Integration

Monitor AND control your Bitaxe mining devices directly in Home Assistant. Full local control with no cloud dependency.

## Before You Start

Before installing this integration:

1. **Home Assistant 2024.1.0 or newer** - This integration requires HA 2024.1.0+.

2. **HACS installed** - This is a custom integration distributed via HACS. See our [HACS installation guide](../brains/install-hacs.md) if you need to install HACS.

3. **Bitaxe running ESP-Miner firmware** - Your device must be running compatible ESP-Miner firmware.

4. **Bitaxe IP address** - The local IP where your Bitaxe is accessible.

## What is Bitaxe?

[Bitaxe](https://bitaxe.org) is an open-source, ASIC-based bitcoin miner designed for home use. Key features:

- **Open hardware** - Fully open-source hardware design
- **Compact** - Small form factor for desktop/home use
- **Local-only** - No cloud services required
- **Customizable** - Adjustable frequency, voltage, and fan settings

## Integration Features

The Exergy Bitaxe integration provides comprehensive monitoring AND control:

### Sensors

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Hashrate | `sensor.bitaxe_hashrate` | Current hashrate |
| Hashrate (1 min avg) | `sensor.bitaxe_hashrate_1m` | 1-minute average hashrate |
| Hashrate (5 min avg) | `sensor.bitaxe_hashrate_5m` | 5-minute average hashrate |
| Hashrate (1 hr avg) | `sensor.bitaxe_hashrate_1h` | 1-hour average hashrate |
| Hashrate (24 hr avg) | `sensor.bitaxe_hashrate_24h` | 24-hour average hashrate |
| Shares Accepted | `sensor.bitaxe_shares_accepted` | Total accepted shares |
| Shares Rejected | `sensor.bitaxe_shares_rejected` | Total rejected shares |
| Error Rate | `sensor.bitaxe_error_rate` | Share error rate percentage |
| Chip Temperature | `sensor.bitaxe_chip_temperature` | ASIC chip temperature |
| VR Temperature | `sensor.bitaxe_vr_temperature` | Voltage regulator temperature |
| Input Voltage | `sensor.bitaxe_input_voltage` | Input voltage |
| Core Voltage | `sensor.bitaxe_core_voltage` | ASIC core voltage (mV) |
| Power | `sensor.bitaxe_power` | Power consumption (W) |
| Fan Speed | `sensor.bitaxe_fan_speed` | Fan speed percentage |
| Uptime | `sensor.bitaxe_uptime` | Device uptime |

### Switches

| Switch | Entity Example | Description |
|--------|----------------|-------------|
| Auto Fan Speed | `switch.bitaxe_auto_fan` | Toggle automatic fan speed control |
| Overclock Enabled | `switch.bitaxe_overclock` | Toggle overclocking mode |
| Invert Screen | `switch.bitaxe_invert_screen` | Invert display colors |

### Number Controls

| Control | Entity Example | Range | Description |
|---------|----------------|-------|-------------|
| Core Voltage | `number.bitaxe_core_voltage` | 1000-1400 mV | ASIC core voltage |
| Frequency | `number.bitaxe_frequency` | 100-1000 MHz | ASIC clock frequency |
| Fan Speed | `number.bitaxe_fan_speed` | 0-100% | Manual fan speed (when auto off) |
| Temp Target | `number.bitaxe_temp_target` | 30-100 C | Target temperature for auto fan |
| Display Timeout | `number.bitaxe_display_timeout` | -1 to 240 min | Screen timeout duration |
| Stats Frequency | `number.bitaxe_stats_frequency` | 0-600 sec | Stats update frequency |

### Select Controls

| Control | Entity Example | Options | Description |
|---------|----------------|---------|-------------|
| Screen Rotation | `select.bitaxe_screen_rotation` | 0, 90, 180, 270 | Display rotation angle |

### Buttons

| Button | Entity Example | Description |
|--------|----------------|-------------|
| Update Firmware | `button.bitaxe_update` | Trigger OTA firmware update |
| Restart | `button.bitaxe_restart` | Restart the device |
| Identify | `button.bitaxe_identify` | Flash screen/LED to identify device |

> **Note:** Entity IDs are generated based on your device name. Find actual IDs at **Settings > Devices & Services > Bitaxe > [your device]**.

## Installation

### Step 1: Install via HACS

1. Open Home Assistant
2. Navigate to **HACS > Integrations**
3. Click **+ Explore & Download Repositories**
4. Search for "Exergy Bitaxe"
5. Click **Download**
6. Restart Home Assistant

### Step 2: Add Your Bitaxe

1. Go to **Settings > Devices & Services**
2. Click **+ Add Integration**
3. Search for "Bitaxe"
4. Enter your configuration:

| Setting | Default | Description |
|---------|---------|-------------|
| **IP Address** | — | Your Bitaxe's local IP address |
| **Port** | 80 | HTTP port (usually 80) |
| **Scan Interval** | 15 sec | How often to poll for updates (5-300 sec) |

5. Click **Submit**

### Step 3: Verify

The integration creates a device with all sensors and controls. Check the device page to see all available entities.

## Finding Your Bitaxe IP Address

### Method 1: Router Admin Panel

1. Log into your router's admin interface
2. Look for "Connected Devices" or "DHCP Leases"
3. Find the device named "Bitaxe" or similar
4. Note the IP address

### Method 2: Bitaxe Web Interface

If you've previously accessed your Bitaxe, check your browser history for the IP.

### Method 3: Network Scanner

Use a network scanning app:
- **Fing** (iOS/Android)
- **Advanced IP Scanner** (Windows)
- **nmap** (Linux/Mac)

**Tip:** Set a static IP or DHCP reservation for your Bitaxe to prevent the address from changing.

## Dashboard Integration

Create a comprehensive Bitaxe control panel:

```yaml
type: vertical-stack
cards:
  - type: entities
    title: Bitaxe Status
    entities:
      - entity: sensor.bitaxe_hashrate
        name: Hashrate
      - entity: sensor.bitaxe_chip_temperature
        name: Chip Temperature
      - entity: sensor.bitaxe_power
        name: Power
      - entity: sensor.bitaxe_shares_accepted
        name: Accepted Shares
      - entity: sensor.bitaxe_error_rate
        name: Error Rate

  - type: entities
    title: Bitaxe Controls
    entities:
      - entity: switch.bitaxe_auto_fan
        name: Auto Fan
      - entity: number.bitaxe_fan_speed
        name: Fan Speed
      - entity: number.bitaxe_frequency
        name: Frequency
      - entity: number.bitaxe_core_voltage
        name: Core Voltage
```

## Automation Ideas

### Temperature Protection

```yaml
automation:
  - alias: "Bitaxe overheat protection"
    trigger:
      - platform: numeric_state
        entity_id: sensor.bitaxe_chip_temperature
        above: 75
    action:
      - service: number.set_value
        target:
          entity_id: number.bitaxe_frequency
        data:
          value: 400
      - service: notify.mobile_app
        data:
          message: "Bitaxe temperature high - reduced frequency"
```

### Scheduled Performance Mode

```yaml
automation:
  - alias: "Bitaxe night performance mode"
    trigger:
      - platform: time
        at: "23:00:00"
    action:
      - service: number.set_value
        target:
          entity_id: number.bitaxe_frequency
        data:
          value: 575

  - alias: "Bitaxe day efficiency mode"
    trigger:
      - platform: time
        at: "07:00:00"
    action:
      - service: number.set_value
        target:
          entity_id: number.bitaxe_frequency
        data:
          value: 450
```

### Low Hashrate Alert

```yaml
automation:
  - alias: "Bitaxe low hashrate alert"
    trigger:
      - platform: numeric_state
        entity_id: sensor.bitaxe_hashrate_1h
        below: 400
        for:
          minutes: 30
    action:
      - service: notify.mobile_app
        data:
          message: "Bitaxe hashrate dropped below 400 GH/s"
```

## Tuning Your Bitaxe

The integration gives you full control over performance parameters:

### Frequency vs Efficiency

| Frequency | Typical Hashrate | Power | Notes |
|-----------|------------------|-------|-------|
| 400 MHz | ~400 GH/s | Low | Most efficient |
| 500 MHz | ~500 GH/s | Medium | Balanced |
| 575 MHz | ~575 GH/s | Higher | Performance |

### Voltage Guidelines

- Start at stock voltage
- Increase in small increments (25mV) if unstable at higher frequencies
- Monitor error rate - keep below 1-2%

### Temperature Management

- Auto fan mode maintains target temperature
- Lower target = more noise, better stability
- Recommended: 55-65C target

## Troubleshooting

### Integration not finding Bitaxe

1. Verify Bitaxe is powered and connected to network
2. Confirm IP address is correct
3. Try accessing Bitaxe web interface directly (`http://[IP_ADDRESS]`)
4. Check that port 80 is not blocked

### Controls not responding

1. Some controls require firmware support
2. Check you're running compatible ESP-Miner firmware
3. Reload the integration after firmware updates

### High error rate

1. Reduce frequency in small steps
2. Increase core voltage slightly
3. Improve cooling
4. Check power supply stability

### Entities showing "Unavailable"

1. Ping the device to confirm it's online
2. Reload the integration: **Settings > Devices & Services > Bitaxe > ⋮ > Reload**
3. Check Home Assistant logs for specific errors

## What's Next?

### Monitor Pool Stats
- [Public Pool Integration](./public-pool.md) - For self-hosted pools
- [Ocean Pool Integration](./ocean-pool.md) - For Ocean pool stats

### Build a Dashboard
- [Dashboard Templates](../dashboards/overview.md) - Pre-built monitoring interfaces

### Set Up Automations
- [Time-of-Use Control](../blueprints/time-of-use.md) - Optimize around electricity rates

## Source Code & Support

- **GitHub:** [github.com/exergyheat/ha-integration-bitaxe](https://github.com/exergyheat/ha-integration-bitaxe)
- **Support Forum:** [support.exergyheat.com](https://support.exergyheat.com)
