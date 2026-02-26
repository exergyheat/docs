<!--explain what home assistant dashboards are, what templates are, and introduce Exergy dashboard templates for hashrate heating-->
# Home Assistant Dashboards

Dashboards are your custom interface to Home Assistant. They're the screens where you see your data, control your devices, and monitor your systems—viewable from the companion mobile app or any web browser.

## What Are Dashboards?

[Integrations](../integrations/overview.md) connect your devices. [Automations](../blueprints/overview.md) make them work together. Dashboards let you see and control everything.

A dashboard is a customizable screen you design:

- **Cards** display data (temperatures, hashrates, power consumption)
- **Buttons** trigger actions (turn on miner, change work mode)
- **Gauges and graphs** visualize trends over time
- **Controls** adjust settings (target temperature, power levels)

Think of it as building your own app interface. You decide exactly what information appears and how it's organized—like designing a custom control panel for your smart home.

## What Are Dashboard Templates?

Dashboard templates are pre-built configurations you can import and customize.

Instead of designing from scratch, you:

1. Import the template YAML
2. Update entity IDs to match your devices
3. Adjust layout and styling to your preference

**Templates vs Dashboards:** A template is a shareable starting point (YAML file). A dashboard is your running interface. One template can be customized into many different dashboards for different setups.

## Why Exergy Makes Dashboard Templates

We design interfaces specifically for hashrate heating systems. Our templates show everything you need in one place:

**Heating controls:**
- Thermostat-style display (target temp, current temp)
- Heating status (active, idle, off)
- Mode selection (heating, mining, eco)

**Miner stats:**
- Real-time hashrate and power consumption
- Temperature readings (intake, exhaust, hashboard)
- Device status and work mode

**Mining earnings:**
- Sats earned today/this month
- Recent payout history
- Pool statistics

It's like a smart thermostat display—but with your bitcoin mining data alongside the temperature controls. One glance tells you if your room is warm, your miner is healthy, and how much you've earned.

## Available Exergy Dashboard Templates

| Template | Use Case |
|----------|----------|
| [Space Heater](./space-heater.md) | Thermostat-style interface for room heating with a miner |
| [HVAC Integration](./hvac.md) | Whole-home system monitoring and control |

**Live Demo:** See a fully configured system at **[demo.exergyheat.com](https://demo.exergyheat.com)**

All templates available on GitHub: **[github.com/exergyheat/docs](https://github.com/exergyheat/docs)**

## Installing Dashboard Templates

Two methods to install any template:

**Copy YAML (fastest)**
1. Go to **Settings → Dashboards** and create a new dashboard
2. Open the raw configuration editor (three dots menu)
3. Paste the template YAML and update entity IDs

**Card by Card**
1. Use the template as a visual reference
2. Add cards manually in the dashboard editor
3. Configure each card with your entities

Each template's dedicated page has detailed installation and customization steps.

## Building Your Own Dashboards

Home Assistant's dashboard editor is powerful and visual:

**Visual Editor** - Drag and drop cards, configure with forms, no code required.

**YAML Mode** - Direct YAML editing for precise control and easy sharing.

**Custom Cards** - Install community cards via HACS for advanced features:
- mini-graph-card for compact history graphs
- mushroom cards for modern, clean styling
- button-card for highly customized controls

**Design tips:** Keep key info visible at a glance. Put common controls within easy reach. Test on both phone and desktop.

## Contributing Dashboard Templates

Created a useful dashboard for hashrate heating? Share it with the community:

- **[Guides Forum](https://support.exergyheat.com/c/guides/6)** - Post screenshots and YAML
- **[GitHub](https://github.com/exergyheat)** - Submit a PR to add your template to the official collection

## Learn More

- **[Home Assistant Dashboard Docs](https://www.home-assistant.io/dashboards/)** - Complete dashboard reference
- **[Exergy Demo](https://demo.exergyheat.com)** - Live example of configured dashboards
- **[Exergy Community Forum](https://support.exergyheat.com/)** - Get help and share ideas
