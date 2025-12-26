<!--explain in a detailed manner what the ocean mining pool integration does, how to install it and use it-->
# Ocean Mining Pool - Exergy Home Assistant Integration

Monitor your Ocean mining pool earnings and statistics directly in Home Assistant.

## Before You Start

Before installing this integration:

1. **Home Assistant with HACS installed** - This is a custom integration distributed via HACS. See our [system configuration guide](../brains/rpi-ha-config.md) if you need to set up Home Assistant and HACS.

2. **Already mining to Ocean** - Your miners must be configured to mine to Ocean pool. The integration pulls data from Ocean's API based on your wallet address.

3. **Your Ocean wallet address** - The bitcoin address you configured in your miner's pool settings.

## What is Ocean?

[Ocean](https://ocean.xyz) is a bitcoin mining pool focused on decentralization and transparency. Key features:

- **Transparent** - Block template selection visible (or build your own with DATUM)
- **No KYC** - Privacy-respecting
- **TIDES payout** - Fair reward distribution (similar to PPLNS)

## Integration Features

The Exergy Ocean integration provides:

### Sensors

| Sensor | Description |
|--------|-------------|
| Mining Account Hashrate (60s) | Current hashrate (60-second average) in TH/s |
| Mining Account Hashrate (300s) | Current hashrate (300-second average) in TH/s |
| Mining Account Shares (60s) | Shares submitted in last 60 seconds |
| Mining Account Shares (300s) | Shares submitted in last 300 seconds |
| Mining Account Shares in Tides | Total shares in TIDES |
| Mining Account Estimated Earnings Next Block | Estimated BTC earnings for next block |
| Mining Account Estimated Bonus Next Block | Estimated bonus BTC for next block |
| Mining Account Estimated Total Earnings Next Block | Total estimated earnings for next block |
| Mining Account Estimated Payout Next Block | Estimated payout for next block |
| Mining Account Unpaid Balance | Unpaid BTC balance |
| Mining Account Unpaid Balance USD | Unpaid balance converted to USD (requires sensor.exchange_rate_1_btc) |
| Mining Account Last Share Timestamp | Timestamp of last submitted share |
| Mining Account Active Workers | Count of currently active workers |
| Mining Account Lifetime Earnings | Total lifetime BTC earnings (scraped from website) |

### Worker Specific Sensors

Workers are **automatically discovered** - if you have 3 workers, you'll get 18 additional sensors (6 per worker). Each worker appears as its own device under the main mining account device. For each worker detected, the following sensors are created dynamically:

| Sensor | Description |
|--------|-------------|
| {worker_name} Hashrate (60s)| Worker's hashrate (60-second average) in TH/s |
| {worker_name} Hashrate (300s) | Worker's hashrate (300-second average) in TH/s |
| {worker_name} Last Share | Timestamp of worker's last submitted share |
| {worker_name} Estimated Earnings Next Block | Worker's estimated BTC earnings for next block |
| {worker_name} Lifetime Earnings | Worker's total lifetime BTC earnings (scraped from website) |
| {worker_name} Status | Connected or Disconnected |

> **Note:** Entity IDs are automatically generated based on your wallet address. Find your actual entity IDs at **Settings → Devices & Services → Exergy Ocean → [your device]**.

## Installation

### Step 1: Install via HACS

1. Open Home Assistant
2. Navigate to **HACS → Integrations**
3. Click **+ Explore & Download Repositories**
4. Search for "Exergy Ocean"
5. Click **Download**
6. Restart Home Assistant

### Step 2: Add Integration

1. Go to **Settings → Devices & Services**
2. Click **+ Add Integration**
3. Search for "Exergy Ocean"
4. Enter your Ocean wallet address
5. Click **Submit**

### Step 3: Verify

The integration creates sensors for your mining statistics. Check the device page to see all available entities.

## Dashboard Integration

Display your mining earnings alongside miner status:

```yaml
type: entities
title: Mining Stats
entities:
  # Entity IDs vary based on your wallet address
  # Find yours at Settings → Devices & Services → Exergy Ocean
  - entity: sensor.ocean_mining_account_hashrate_60s
    name: My Hashrate
  - entity: sensor.ocean_mining_account_estimated_earnings_next_block
    name: Est. Earnings
  - entity: sensor.ocean_mining_account_unpaid_balance
    name: Unpaid Balance
```

## Automation Ideas

### Low Hashrate Alert

Notify when your pool hashrate drops (indicates miner issues):

```yaml
automation:
  - alias: "Low hashrate alert"
    trigger:
      - platform: numeric_state
        entity_id: sensor.ocean_hashrate_24h
        below: 30  # TH/s threshold
        for:
          hours: 2
    action:
      - service: notify.mobile_app
        data:
          message: "Mining hashrate dropped below threshold"
```

### Daily Earnings Summary

Send a daily summary of mining earnings:

```yaml
automation:
  - alias: "Daily mining summary"
    trigger:
      - platform: time
        at: "20:00:00"
    action:
      - service: notify.mobile_app
        data:
          message: >
            Today's mining: ~{{ states('sensor.ocean_estimated_daily') }} sats
            Unpaid: {{ states('sensor.ocean_unpaid_balance') }} sats
```

## Finding Your Ocean Wallet Address

Your Ocean wallet address is the bitcoin address you configured when setting up your miner to point to Ocean:

1. This is the address in your miner's pool configuration
2. It's also visible at `ocean.xyz/[your-address]`

## Troubleshooting

### No data appearing

1. Verify wallet address is correct
2. Ensure miners are actively mining to Ocean
3. Wait for data to populate (can take a few hours for new setups)
4. Check Ocean website directly to verify mining activity

### Hashrate discrepancy

- Pool-reported hashrate is averaged over time
- Local miner hashrate is real-time
- Some variation is normal
- Significant differences may indicate connectivity issues

## What's Next?

### Connect Your Miners
To mine to Ocean, you need miners connected to Home Assistant:
- [Canaan Avalon Home Integration](./exergy-canaan.md) - Connect Avalon miners

### Build a Dashboard
Ocean sensors work great alongside miner stats:
- [Space Heater Dashboard](../dashboards/space-heater.md) - Includes mining earnings display
- [HVAC Dashboard](../dashboards/hvac.md) - System monitoring with earnings

## Resources

- [Ocean Pool](https://ocean.xyz)
- [Ocean Documentation](https://ocean.xyz/docs)
- [Exergy GitHub](https://github.com/exergyheat)
