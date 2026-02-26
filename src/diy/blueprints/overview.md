<!--explain what home assistant automations are, what blueprints are, and introduce Exergy blueprints for hashrate heating-->
# Home Assistant Automations and Templates<!-- & Blueprints-->

Automations are the smart logic that makes your smart home actually smart. They connect your [integrations](../integrations/overview.md) together—triggering actions based on sensor readings, schedules, or device states. This is what transforms a bitcoin miner into a functional heater for your home.

## What Are Automations?

Integrations connect your devices to Home Assistant. Automations make them work together.

Every automation follows the same pattern:

- **Trigger** - What starts the automation (temperature drops, time of day, button press)
- **Condition** - Optional checks before proceeding (only if home, only on weekdays)
- **Action** - What happens (turn on miner, adjust power level, send notification)

**Example:** When room temperature drops below 68°F (trigger), and it's between 6am-10pm (condition), turn on the miner in heating mode (action).

This is the special sauce that lets a bitcoin miner replace your space heater, tie into your whole-home thermostat, or only run when electricity is cheap.

<!--## What Are Blueprints?

Blueprints are reusable automation templates.

Instead of building automation logic from scratch, you:

1. Import a pre-built blueprint
2. Fill in your specific entities (your miner, your thermostat, your temperature sensor)
3. Adjust settings to your preferences
4. Activate

**Blueprints vs Automations:** A blueprint is a template. An automation is a running instance. One blueprint can create many automations with different settings—like using the same recipe to cook for different numbers of guests.

## Why Exergy Makes Blueprints

We build blueprints for common hashrate heating scenarios so you don't have to figure out the automation logic yourself.
-->
**Use cases we cover:**

- **Space heater replacement** - Miner maintains room temperature using a temp sensor
- **HVAC integration** - Miner responds to your whole-home thermostat
- **Time-of-use optimization** - Mine during cheap electricity, reduce during peak rates
- **Solar monetization** - Use excess solar production for mining instead of selling back cheap
- **Scheduled operation** - Run on custom schedules with different modes

These automations pair with Exergy hardware kits and installed systems, but work with any compatible setup.

## Available Exergy Automation Templates<!--Blueprints-->

| Template <!--Blueprint--> | Use Case |
|-----------|----------|
| [Space Heater](./space-heater.md) | Room temperature control with miner as heat source |
| [HVAC Integration](./hvac.md) | Whole-home thermostat integration |
| [Time-of-Use](./time-of-use.md) | Optimize mining around electricity rates |

See all Exergy Automations<!-- blueprints-->: **[github.com/exergyheat/docs](https://github.com/exergyheat/docs)**

<!--## Installing Blueprints

Two methods to install any blueprint:

**Import URL (easiest)**
1. Go to **Settings → Automations & Scenes → Blueprints**
2. Click **Import Blueprint** and paste the GitHub URL

**Manual YAML**
1. Download the blueprint file
2. Place in `/config/blueprints/automation/exergy/`
3. Restart Home Assistant

Each blueprint's dedicated page has detailed installation and configuration steps.
-->
## Building Your Own Automations

Don't see what you need? Build custom automations:

**Visual Editor** - Home Assistant's built-in editor walks you through triggers, conditions, and actions without code.

**YAML** - Write automations directly for more control and easier version management.

**Node-RED** - Visual flow-based programming for complex logic. Install via the Node-RED add-on.

Home Assistant's automation system is powerful. Start with our blueprints, then customize or build your own as you learn.

## Contributing Blueprints and Automations

Created a useful automation for hashrate heating? Share it with the community:

- **[Guides Forum](https://support.exergyheat.com/c/guides/6)** - Post your automation for discussion
- **[GitHub](https://github.com/exergyheat)** - Submit a PR to add your blueprint to the official collection

## Learn More

- **[Home Assistant Automation Docs](https://www.home-assistant.io/docs/automation/)** - Complete automation reference
- **[Home Assistant Blueprint Docs](https://www.home-assistant.io/docs/automation/using_blueprints/)** - Blueprint usage and creation
- **[Exergy Community Forum](https://support.exergyheat.com/)** - Get help and share ideas
