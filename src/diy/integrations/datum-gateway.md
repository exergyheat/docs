<!--explain in a detailed manner what the datum gateway integration does, how to install it and use it-->
# DATUM Gateway - Exergy Home Assistant Integration

Monitor your DATUM Gateway bitcoin mining proxy directly in Home Assistant. Track hashrate, shares, pool status, and block template information.

## Before You Start

Before installing this integration:

1. **Home Assistant 2023.1.0 or newer** - This integration requires HA 2023.1.0+.

2. **HACS installed** - This is a custom integration distributed via HACS. See our [system configuration guide](../brains/rpi-ha-config.md) if you need to set up HACS.

3. **DATUM Gateway running** - You need a DATUM Gateway instance running v0.4.0 or higher.

4. **Gateway URL** - The URL where your DATUM Gateway is accessible.

## What is DATUM Gateway?

[DATUM Gateway](https://github.com/OCEAN-xyz/datum_gateway) is self-hosted mining proxy software that enables miners to construct their own block templates while still mining to a pool. Key features:

- **Block template construction** - Build your own templates instead of relying solely on pool-provided ones
- **Decentralization** - Reduces pool control over transaction selection
- **Transparency** - Full visibility into block template details
- **Ocean integration** - Works seamlessly with Ocean mining pool

DATUM stands for "Decentralized Alternative Templates for Universal Mining."

## Integration Features

The Exergy DATUM Gateway integration provides monitoring of your gateway operations:

### Sensors

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Combined Hashrate | `sensor.datum_gateway_hashrate` | Total hashrate from all connected miners |
| Thread Count | `sensor.datum_gateway_thread_count` | Number of active mining threads |
| Shares Accepted | `sensor.datum_gateway_shares_accepted` | Total accepted shares |
| Shares Rejected | `sensor.datum_gateway_shares_rejected` | Total rejected shares |
| Acceptance Rate | `sensor.datum_gateway_acceptance_rate` | Share acceptance percentage |
| Pool Status | `sensor.datum_gateway_pool_status` | Connection status to mining pool |
| Block Height | `sensor.datum_gateway_block_height` | Current block template height |
| Block Reward | `sensor.datum_gateway_block_reward` | Current block reward (BTC) |
| Transaction Count | `sensor.datum_gateway_tx_count` | Transactions in current block template |
| Uptime | `sensor.datum_gateway_uptime` | Gateway process uptime |
| Pool Endpoint | `sensor.datum_gateway_pool_endpoint` | Connected pool URL |
| Miner Tag | `sensor.datum_gateway_miner_tag` | Configured miner tag |

### Per-Thread Sensors

For each connected mining thread, additional sensors are created:

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Thread Hashrate | `sensor.datum_gateway_thread_X_hashrate` | Individual thread hashrate |
| Thread Shares | `sensor.datum_gateway_thread_X_shares` | Thread share count |

> **Note:** Entity IDs are generated based on your gateway configuration. Find actual IDs at **Settings > Devices & Services > DATUM Gateway > [your device]**.

## Installation

### Step 1: Install via HACS

1. Open Home Assistant
2. Navigate to **HACS > Integrations**
3. Click **+ Explore & Download Repositories**
4. Search for "DATUM Gateway" or "Exergy DATUM"
5. Click **Download**
6. Restart Home Assistant

### Step 2: Add Integration

1. Go to **Settings > Devices & Services**
2. Click **+ Add Integration**
3. Search for "DATUM Gateway"
4. Enter your configuration:

| Setting | Default | Description |
|---------|---------|-------------|
| **Gateway URL** | — | Full URL to your DATUM Gateway |
| **SSL Verification** | On | Toggle SSL certificate verification |

5. Click **Submit**

### Step 3: Verify

The integration creates sensors for your gateway statistics. Check the device page to see all available entities.

## Configuration

### Gateway URL

The URL must include the protocol and port:

- **Local:** `http://192.168.1.100:7152`
- **Remote with SSL:** `https://datum.example.com:7152`
- **Docker:** `http://datum-gateway:7152`

### SSL Verification

Disable SSL verification only if using self-signed certificates. For production deployments, use valid certificates.

## Dashboard Integration

Display your DATUM Gateway stats:

```yaml
type: vertical-stack
cards:
  - type: entities
    title: DATUM Gateway Status
    entities:
      - entity: sensor.datum_gateway_hashrate
        name: Gateway Hashrate
      - entity: sensor.datum_gateway_pool_status
        name: Pool Status
      - entity: sensor.datum_gateway_acceptance_rate
        name: Acceptance Rate
      - entity: sensor.datum_gateway_thread_count
        name: Active Threads

  - type: entities
    title: Block Template
    entities:
      - entity: sensor.datum_gateway_block_height
        name: Block Height
      - entity: sensor.datum_gateway_block_reward
        name: Block Reward
      - entity: sensor.datum_gateway_tx_count
        name: Transactions
```

## Automation Ideas

### Block Height Change Notification

```yaml
automation:
  - alias: "New block notification"
    trigger:
      - platform: state
        entity_id: sensor.datum_gateway_block_height
    action:
      - service: notify.mobile_app
        data:
          message: "New block! Height: {{ states('sensor.datum_gateway_block_height') }}"
```

### Pool Disconnect Alert

```yaml
automation:
  - alias: "DATUM pool disconnect alert"
    trigger:
      - platform: state
        entity_id: sensor.datum_gateway_pool_status
        to: "disconnected"
        for:
          minutes: 5
    action:
      - service: notify.mobile_app
        data:
          message: "DATUM Gateway lost pool connection"
```

### Low Acceptance Rate Alert

```yaml
automation:
  - alias: "DATUM low acceptance rate"
    trigger:
      - platform: numeric_state
        entity_id: sensor.datum_gateway_acceptance_rate
        below: 95
        for:
          minutes: 10
    action:
      - service: notify.mobile_app
        data:
          message: "DATUM acceptance rate below 95%: {{ states('sensor.datum_gateway_acceptance_rate') }}%"
```

### Hashrate Drop Alert

```yaml
automation:
  - alias: "DATUM hashrate drop"
    trigger:
      - platform: numeric_state
        entity_id: sensor.datum_gateway_hashrate
        below: 50
        for:
          minutes: 15
    action:
      - service: notify.mobile_app
        data:
          message: "DATUM Gateway hashrate dropped below 50 TH/s"
```

## Troubleshooting

### No data appearing

1. Verify Gateway URL is correct and accessible
2. Ensure DATUM Gateway is running v0.4.0 or higher
3. Check Home Assistant logs for connection errors
4. Try accessing the gateway status page directly in a browser

### SSL Errors

1. Verify the SSL certificate is valid
2. Try disabling SSL verification temporarily to diagnose
3. Ensure the URL protocol matches your setup (http vs https)

### Dependency Issues

This integration requires BeautifulSoup4. If you encounter import errors:
1. Check Home Assistant logs for specific error messages
2. Restart Home Assistant to trigger dependency installation

### Stale Data

1. Check gateway is actively receiving work from miners
2. Verify pool connection is healthy
3. Reload the integration to force a refresh

## What's Next?

### Connect Your Miners
- [Canaan Avalon Integration](./exergy-canaan.md) - Connect Avalon miners
- [Bitaxe Integration](./bitaxe.md) - Connect Bitaxe miners
- [StealthMiner/LuxOS Integration](./stealthminer.md) - Connect LuxOS miners

### Monitor Pool Stats
- [Ocean Pool Integration](./ocean-pool.md) - Monitor Ocean pool earnings

### Build a Dashboard
- [Dashboard Templates](../dashboards/overview.md) - Pre-built monitoring interfaces

## Resources

- [DATUM Gateway GitHub](https://github.com/OCEAN-xyz/datum_gateway)
- [Ocean Pool](https://ocean.xyz)
- [Exergy GitHub](https://github.com/exergyheat)
- [Exergy Community Forum](https://support.exergyheat.com)

## Source Code & Support

- **GitHub:** [github.com/exergyheat/ha-integration-datum-gateway](https://github.com/exergyheat/ha-integration-datum-gateway)
- **Support Forum:** [support.exergyheat.com](https://support.exergyheat.com)
