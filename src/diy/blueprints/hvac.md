<!--explain in a detailed manner what this automation does, how to download and configure it, and how to customize and expand upon it-->
# HVAC Integrated Thermostat Control - Exergy HA Blueprint

Integrate your bitcoin miner into your home's HVAC system as "Stage 1" heating, with your existing furnace as backup.

## Before You Start

- **Miner connected to Home Assistant** - Install the [Canaan Avalon Home Integration](../integrations/exergy-canaan.md) first
- **Smart thermostat** - Connected to HA (Z-Wave, Zigbee, or WiFi)
- **Home Assistant with HACS** - See our [system configuration guide](../brains/rpi-ha-config.md) if needed
- **Professional installation** - Ducting and wiring modifications recommended

## How It Works

This setup uses your miner as the primary heat source:

1. **Stage 1 (Miner)**: When heating is needed, miner turns on first
2. **Stage 2 (Furnace)**: If miner can't keep up, furnace activates
3. **Miner produces bitcoin** while providing base heating
4. **Furnace only runs** when additional heat is needed

## System Architecture

```
                    ┌─────────────────┐
                    │   Thermostat    │
                    │  (Smart/Multi)  │
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
         ▼                   ▼                   ▼
  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
  │  Stage 1     │   │  Stage 2     │   │  Cooling     │
  │  (Miner)     │   │  (Furnace)   │   │  (AC)        │
  │  via HA      │   │  Direct wire │   │  Direct wire │
  └──────────────┘   └──────────────┘   └──────────────┘
```

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
| Thermostat Entity | Your HVAC thermostat |
| Stage Sensor | Entity that reports which heating stage is active |

### Optional Inputs

| Input | Default | Description |
|-------|---------|-------------|
| Stage 1 Delay | 30 sec | Wait time before turning on miner after Stage 1 call |
| Shutdown Delay | 60 sec | Wait time before turning off miner after call ends |

## Wiring Considerations

### Traditional Thermostat Wiring

**Note**: Our recommended thermostat is a Venstar T7900 for its ability to be controlled via local APIs without the need for Cloud interface or third party servers

Standard HVAC wiring:
- **W1**: Stage 1 heat (becomes miner via HA)
- **W2**: Stage 2 heat (remains wired to furnace)
- **Y**: Cooling
- **G**: Fan
- **C**: Common (power)

### Modification Approach

1. Disconnect W1 from furnace control board
2. W1 now only signals Home Assistant (via smart thermostat)
3. W2 remains connected to furnace
4. HA controls miner based on W1 signal

**This is just an example. Please consult an HVAC professional for your specific system.**

## Example Configuration

```yaml
automation:
  - alias: Control switch based on upstairs thermostat heating
    description: Turn switch on/off based on active heating state
    triggers:
      - entity_id: climate.thermostat
        attribute: hvac_action
        to: heating
        id: heating_started
        trigger: state
      - entity_id: climate.thermostat
        attribute: hvac_action
        from: heating
        id: heating_stopped
        trigger: state
    conditions: []
    actions:
      - choose:
          - conditions:
              - condition: trigger
                id: heating_started
            sequence:
              - action: switch.turn_on
                data: {}
                target:
                  entity_id: switch.avalon_q_power
          - conditions:
              - condition: trigger
                id: heating_stopped
            sequence:
              - action: switch.turn_off
                data: {}
                target:
                  entity_id: switch.avalon_q_power
    mode: restart
```

## Airflow Requirements

### Duct Sizing

- Match miner CFM output to duct capacity
- Avalon Q: ~200 CFM typical
- Ensure no backpressure on miner fans

### Plenum Connection

- Connect to supply plenum (after furnace heat exchanger)
- Use insulated flex duct
- Include damper for isolation when miner is off

### Safety Considerations

- Install high-limit safety switch
- Ensure proper condensate drainage
- CO detector near furnace (as always)

## Performance Expectations

### Typical Savings

With an Avalon Q (~5,630 BTU/hr):
- Covers ~30-50% of heating needs in moderate climates
- Furnace runs significantly less
- Bitcoin earnings offset electricity costs

### Best Results

- Well-insulated homes
- Moderate heating climates
- Time-of-use electricity rates (mine during cheap hours)

## Troubleshooting

### Miner not turning on

- Verify thermostat is reporting heating mode to HA
- Check automation triggers are correct
- Confirm miner is available in HA

### Furnace activating too often

- Miner may not provide enough heat
- Increase target temperature differential
- Consider additional miner or higher power mode

### Short cycling

- Add delays to automations
- Increase thermostat hysteresis
- Check airflow through miner

## Dashboard Integration

See [HVAC Integrated Wall+Digital Thermostat](../dashboards/hvac.md) for a matching dashboard to monitor this setup.

## Resources

- [Home Assistant Automation Docs](https://www.home-assistant.io/docs/automation/)
- [Generic Thermostat Integration](https://www.home-assistant.io/integrations/generic_thermostat/)
- [Exergy GitHub](https://github.com/exergyheat)
