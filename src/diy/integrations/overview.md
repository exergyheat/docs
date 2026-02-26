<!--explain what home assistant integrations are, official vs custom integrations, and introduce Exergy integrations for bitcoin miners-->
# Home Assistant Integrations

Integrations are how Home Assistant connects to your devices. Every smart device in your home - thermostats, lights, security cameras, and now bitcoin miners - uses its own platform or protocol. Integrations bring them all together into one system.

## What Are Integrations?

Your smart thermostat has its own app. Your lights have another. Your security system has a third. Each device lives in its own silo with its own interface.

Home Assistant integrations solve this by acting as connectors. Each integration:

- **Discovers or connects** to a specific device or service
- **Creates entities** (sensors, switches, controls) you can use in Home Assistant
- **Keeps data in sync** between the device and Home Assistant

Integrations handle the connection. Once your devices are connected, [automations](../blueprints/overview.md) handle the smart logic (when to turn things on, what triggers what), and [dashboards](../dashboards/overview.md) provide your control interface.

## Official vs Custom Integrations

### Official Integrations

Home Assistant includes over 2,000 official integrations out of the box. These are:

- Built into Home Assistant core
- Maintained by Home Assistant team or Meet Home Assistant team criterea for offical integrations
- Configured through **Settings → Devices & Services**

Common examples: Google Cast, Philips Hue, MQTT, Zigbee (ZHA), weather services.

### Custom Integrations (HACS)

HACS (Home Assistant Community Store) extends Home Assistant with community-developed integrations not included in the core.

- **Community-built** - Developers create integrations for devices not officially supported
- **Easy installation** - HACS provides a store-like interface for discovery and updates
- **Where Exergy integrations *currently* live** - Our bitcoin miner integrations are available through HACS

To use custom integrations, you first install HACS, documented [here](../brains/install-hacs.md), then browse and install the integrations you need.

## Common Integrations

People add integrations for all their smart devices:

| Category | Examples |
|----------|----------|
| Climate | Nest, Ecobee, Honeywell thermostats |
| Lighting | Philips Hue, Lutron, LIFX |
| Security | Ring, Arlo, Wyze cameras |
| Media | Sonos, Roku, Apple TV |
| Energy | Sense, Emporia, utility meters |
| Protocols | Zigbee (ZHA), Z-Wave, Matter |
| Weather | OpenWeatherMap, Met.no, NWS |

Browse all official integrations at [home-assistant.io/integrations](https://www.home-assistant.io/integrations/).

Browse all custom HACS integrations on the [HACS Github](https://github.com/hacs/default/blob/master/integration).

## Exergy Integrations

We built integrations to bring bitcoin miners into the smart home ecosystem. Connect, control, and monitor your home mining hardware just like any other IoT device.

### Miner Integrations

| Integration | Purpose |
|-------------|---------|
| [Canaan Avalon Home](./exergy-canaan.md) | Control & monitor Canaan Avalon miners (Mini 3, Q, Nano 3s) |
| [Bitaxe](./bitaxe.md) | Monitor & control Bitaxe open-source miners |
| [StealthMiner / LuxOS](./stealthminer.md) | Monitor & control miners with LuxOS firmware |

### Pool & Infrastructure Integrations

| Integration | Purpose |
|-------------|---------|
| [Ocean Mining Pool](./ocean-pool.md) | Monitor Ocean pool earnings and statistics |
| [Public Pool](./public-pool.md) | Monitor self-hosted Public Pool instances |
| [DATUM Gateway](./datum-gateway.md) | Monitor DATUM Gateway block template proxy |

See a full list of Exergy Integrations here:

[Index of Exergy's Home Assistant Integrations](https://github.com/exergyheat/ha-integrations-index)

## GitHub

All Exergy integrations are open source:

**[github.com/exergyheat](https://github.com/exergyheat)**

Contributions welcome—bug reports, feature requests, and code contributions.
