<!--explain the high level overview of the DIY section of the docs website-->
# DIY Overview

The core piece of a sovereign smart home is a physical computer (home brain) that lives in your building, running 24/7 to manage all smart devices. 

Hashrate heaters, small desktop bitcoin miners, thermostats, lights, alarms, blinds, etc... are all smart devices that need to connect to the home brain to talk to one another and operate as a cohesive system.

This section walks through our open-source documentation on how to build and run your own self-hosted smart home brain, integrate bitcoin miners and hashrate heaters into the broader home IoT network, and build automations and dashboards that help you take advantage of hashrate heating.

## Why DIY?

- **Learn** - Understand, inspect and verify every component of your system
- **Customize** - Build exactly what you need, nothing more
- **Save** - Use hardware you already own
- **Self-sovereign** - No reliance on any specific vendor or service provider

## Sovereign Smart Home Brains

### 1. Home Assistant
[Home Assistant](https://www.home-assistant.io) OS (HAOS) is a minimalistic, embedded operating system optimized for running the Home Assistant smart home platform, providing a robust, maintenance-free environment for local automation on devices like single-board computers or dedicated hardware.

| Section | Description |
|---------|-------------|
| [Build Your Own HA Brain](./brains/overview.md) | Build and set up your Home Assistant smart device control brain on various hardware systems |
| [HA Integrations](./integrations/overview.md) | Use Exergy integration applications to connect  bitcoin miners and more to your smart home |
| [HA Blueprints](./blueprints/overview.md) | Automation templates that define when things run, what they connect to, and more |
| [HA Dashboards](./dashboards/overview.md) | Visualize your sovereign smart home and control everything remotely |

#### Home Assistant DIY Overview

##### Step 1: Set Up Home Assistant
[Raspberry Pi](./diy/brains/rpi-ha.md)

- Build the required hardware and install Home Assistant. Complete the setup and configure get your home brain ready for hashrate heating.

*Exergy Home Assistant Documentation for StartOS and Umbrel servers coming soon.*

##### Step 2: Set Up Hashrate Heating Hardware

- Choose your favorite bitcoin mining heater (Avalon Mini 3, Avalon Q, or Nano 3s), follow the initial device setup and connect it to your local network.

##### Step 3: Add Integrations
[Exergy HA Integrations](./diy/integrations/overview.md)

- Install the Exergy integrations (like applications on HA) to easily connect and manage your hashrate heaters in your sovereign smart home, with real time control and data read outs.

##### Step 4: Configure Automations
[Exergy HA Automation Blueprints](./diy/blueprints/overview.md)

- Use our blueprints as a starting point, or build your own automations from scratch. Tie together heat, thermostats, miners, temperature sensors, timing, and more.

##### Step 5: Build Your Dashboard
[Exergy HA Dashboard Templates](./diy/dashboards/overview.md)

- Create a control interface using our templates or design your own. Visualize everything at a glance and control from anywhere.

## GitHub

Find all open-source Exergy code on our Github:

**[github.com/exergyheat](https://github.com/exergyheat)**

### Contributing

We welcome contributions:

- Bug reports and fixes
- New automation blueprints
- Dashboard improvements
- Documentation updates

See individual GitHub repositories for contribution guidelines.

## Community

- **[Community Guides Forum](https://support.exergyheat.com/c/guides/6)** - Share your smart home automations, dashboards and discover tips
- **[Software Support Forum](https://support.exergyheat.com/c/sw-support/14)** - Get help with specific issues
- **[GitHub Issues](https://github.com/exergyheat)** - Report bugs, request features, submit PRs
