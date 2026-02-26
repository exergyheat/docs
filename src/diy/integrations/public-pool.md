<!--explain in a detailed manner what the public pool integration does, how to install it and use it-->
# Public Pool - Exergy Home Assistant Integration

Monitor your self-hosted Public Pool instance directly in Home Assistant. Compatible with Start9 and other self-hosted deployments.

## Before You Start

Before installing this integration:

1. **Home Assistant with HACS installed** - This is a custom integration distributed via HACS. See our [system configuration guide](../brains/rpi-ha-config.md) if you need to set up Home Assistant and HACS.

2. **Self-hosted Public Pool running** - You need a Public Pool instance running (self-hosted or Start9).

3. **Pool URL** - The URL where your Public Pool instance is accessible.

4. **Your Bitcoin address** - The address you're mining to (for address-level stats).

## What is Public Pool?

[Public Pool](https://github.com/benjamin-wilson/public-pool) is open-source mining pool software designed for self-hosting. Key features:

- **Self-sovereign** - Run your own pool infrastructure
- **Start9 compatible** - Easy deployment on Start9 servers
- **Privacy-focused** - Your data stays on your infrastructure
- **Solo mining** - Mine blocks directly to your wallet
- **No fees** - You control everything

## Integration Features

The Exergy Public Pool integration provides multi-level monitoring:

### Pool-Level Sensors

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Pool Hashrate | `sensor.public_pool_hashrate` | Total pool hashrate in TH/s |
| Miner Count | `sensor.public_pool_miner_count` | Number of connected miners |
| Block Height | `sensor.public_pool_block_height` | Current block being worked on |

### Network-Level Sensors

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Network Difficulty | `sensor.public_pool_network_difficulty` | Current bitcoin network difficulty |
| Network Hashrate | `sensor.public_pool_network_hashrate` | Network hashrate in EH/s |
| Blockchain Height | `sensor.public_pool_blockchain_height` | Current blockchain height |

### Address-Level Sensors

Sensors for your specific mining address:

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Best Difficulty | `sensor.public_pool_address_best_difficulty` | Best difficulty share found |
| Worker Count | `sensor.public_pool_address_worker_count` | Number of workers for this address |
| Address Hashrate | `sensor.public_pool_address_hashrate` | Your address hashrate in GH/s |

### Per-Worker Sensors

For each worker detected, these sensors are created dynamically:

| Sensor | Entity Example | Description |
|--------|----------------|-------------|
| Worker Hashrate | `sensor.public_pool_worker_{name}_hashrate` | Worker hashrate in GH/s |
| Worker Best Difficulty | `sensor.public_pool_worker_{name}_best_difficulty` | Worker's best difficulty share |
| Worker Last Activity | `sensor.public_pool_worker_{name}_last_activity` | Timestamp of last activity |

> **Note:** Entity IDs are automatically generated. Find your actual entity IDs at **Settings > Devices & Services > Public Pool > [your device]**.

## Installation

### Step 1: Install via HACS

1. Open Home Assistant
2. Navigate to **HACS > Integrations**
3. Click **+ Explore & Download Repositories**
4. Search for "Public Pool" or "Exergy Public Pool"
5. Click **Download**
6. Restart Home Assistant

### Step 2: Add Integration

1. Go to **Settings > Devices & Services**
2. Click **+ Add Integration**
3. Search for "Public Pool"
4. Enter your configuration:

| Setting | Default | Description |
|---------|---------|-------------|
| **Pool URL** | — | URL to your Public Pool instance |
| **Bitcoin Address** | — | Your mining address for address-level stats |
| **Scan Interval** | 60 sec | How often to poll for updates |
| **SSL Verification** | On | Toggle SSL certificate verification |

5. Click **Submit**

### Step 3: Verify

The integration creates sensors at pool, network, address, and worker levels. Check the device page to see all available entities.

## Configuration

### Pool URL Examples

- **Local Start9:** `http://public-pool.embassy:3334`
- **Local network:** `http://192.168.1.100:3334`
- **Remote with SSL:** `https://pool.example.com`
- **Tor (Start9):** Use local embassy address for better performance

### Bitcoin Address

Enter the address your miners are configured to mine to. This enables:
- Address-specific hashrate tracking
- Best difficulty monitoring
- Per-worker statistics

### Scan Interval

Default is 60 seconds. Range: 30-300 seconds.

- Lower intervals = more responsive but more API load
- Higher intervals = less load but delayed updates

## Start9 Integration

If running Public Pool on Start9:

1. Find your pool's local address in Start9's interface
2. Use the local embassy address: `http://public-pool.embassy:3334`
3. This avoids Tor latency for local access
4. For remote access, configure Tor proxy settings

## Dashboard Integration

Display your Public Pool stats:

```yaml
type: vertical-stack
cards:
  - type: entities
    title: My Public Pool
    entities:
      - entity: sensor.public_pool_hashrate
        name: Pool Hashrate
      - entity: sensor.public_pool_miner_count
        name: Connected Miners
      - entity: sensor.public_pool_address_hashrate
        name: My Hashrate
      - entity: sensor.public_pool_address_best_difficulty
        name: Best Difficulty

  - type: entities
    title: Network Stats
    entities:
      - entity: sensor.public_pool_network_difficulty
        name: Network Difficulty
      - entity: sensor.public_pool_network_hashrate
        name: Network Hashrate
      - entity: sensor.public_pool_blockchain_height
        name: Block Height
```

## Automation Ideas

### Worker Offline Alert

```yaml
automation:
  - alias: "Public Pool worker offline"
    trigger:
      - platform: template
        value_template: >
          {{ (now() - states.sensor.public_pool_worker_miner1_last_activity.last_changed).total_seconds() > 600 }}
    action:
      - service: notify.mobile_app
        data:
          message: "Worker miner1 appears offline - no activity for 10 minutes"
```

### New Best Difficulty Celebration

```yaml
automation:
  - alias: "New best difficulty found"
    trigger:
      - platform: state
        entity_id: sensor.public_pool_address_best_difficulty
    condition:
      - condition: template
        value_template: "{{ trigger.to_state.state | float > trigger.from_state.state | float }}"
    action:
      - service: notify.mobile_app
        data:
          message: "New best difficulty: {{ states('sensor.public_pool_address_best_difficulty') }}!"
```

### Low Hashrate Alert

```yaml
automation:
  - alias: "Public Pool low hashrate"
    trigger:
      - platform: numeric_state
        entity_id: sensor.public_pool_address_hashrate
        below: 100
        for:
          minutes: 30
    action:
      - service: notify.mobile_app
        data:
          message: "Public Pool hashrate dropped below 100 GH/s"
```

### Daily Mining Summary

```yaml
automation:
  - alias: "Daily Public Pool summary"
    trigger:
      - platform: time
        at: "20:00:00"
    action:
      - service: notify.mobile_app
        data:
          message: >
            Public Pool Stats:
            Hashrate: {{ states('sensor.public_pool_address_hashrate') }} GH/s
            Best Diff: {{ states('sensor.public_pool_address_best_difficulty') }}
            Workers: {{ states('sensor.public_pool_address_worker_count') }}
```

## Solo Mining Considerations

Public Pool is designed for solo mining:

- **Variance is high** - You may go long periods without finding a block
- **Best difficulty matters** - Track your progress toward a block
- **Patience required** - Small hashrate means low probability per block

### Probability Calculations

Your chance of finding a block depends on:
- Your hashrate vs network hashrate
- Current network difficulty
- Time

At 1 TH/s against ~600 EH/s network, expect very long times between blocks. Public Pool is best suited for users who:
- Value sovereignty over consistent payouts
- Have significant hashrate
- Want to learn about mining without third-party pools

## Troubleshooting

### No data appearing

1. Verify Pool URL is correct and accessible
2. Check that your bitcoin address is actively mining
3. Wait for workers to appear (they show after mining activity)
4. Check Home Assistant logs for API errors

### Workers not showing

1. Workers appear dynamically based on mining activity
2. Inactive workers may not appear immediately
3. Verify workers are configured with the correct bitcoin address
4. Check miner is actually submitting work

### SSL/Certificate errors

1. Start9 uses self-signed certificates by default
2. Disable SSL verification if using self-signed certs
3. Use local embassy address to avoid certificate issues

### Stale data

1. Check pool is receiving work from miners
2. Verify scan interval isn't too long
3. Reload the integration to force refresh

## What's Next?

### Connect Your Miners
- [Bitaxe Integration](./bitaxe.md) - For Bitaxe miners
- [Canaan Avalon Integration](./exergy-canaan.md) - For Avalon home miners
- [StealthMiner/LuxOS Integration](./stealthminer.md) - For LuxOS firmware miners

### Build a Dashboard
- [Dashboard Templates](../dashboards/overview.md) - Pre-built monitoring interfaces

## Resources

- [Public Pool GitHub](https://github.com/benjamin-wilson/public-pool)
- [Start9 Documentation](https://start9.com)
- [Exergy Docs on GitHub](https://github.com/exergyheat/docs)
- [Exergy Community Forum](https://support.exergyheat.com)

## Source Code & Support

- **GitHub:** [github.com/exergyheat/ha-integration-public-pool](https://github.com/exergyheat/ha-integration-public-pool)
- **Support Forum:** [support.exergyheat.com](https://support.exergyheat.com)
