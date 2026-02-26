# Install HACS

HACS (Home Assistant Community Store) is required for installing Exergy custom integrations. HACS is not included with Home Assistant by default and must be installed once, regardless of what hardware your Home Assistant runs on.

---

## Prerequisites

Before installing HACS:
- Home Assistant version **2024.4.1 or newer** (Settings → About to check)
- A **GitHub account** (free) - [Sign up at github.com](https://github.com/signup)
- Stable internet connection

## Install the Get HACS Add-on

1. **Open the add-on repository link**
   - Click this link from your Home Assistant browser session: [Add HACS Repository](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fhacs%2Faddons)
   - Or manually: Go to **Settings → Add-ons → Add-on Store** (three dots menu) → **Repositories** → Add: `https://github.com/hacs/addons`

2. **Install the Get HACS add-on**
   - In the Add-on Store, search for **Get HACS**
   - Click on it, then click **Install**

3. **Start the add-on**
   - After installation, click **Start**
   - Click the **Log** tab to see the output
   - Follow any instructions shown in the log

4. **Restart Home Assistant**
   - Go to **Settings → System → Restart**
   - Click **Restart** and wait for Home Assistant to come back online

## Configure HACS

1. **Add the HACS integration**
   - Go to **Settings → Devices & Services**
   - Click **+ Add Integration** (bottom right)
   - Search for **HACS** and select it
   - If HACS doesn't appear, clear your browser cache and try again

2. **Accept the terms**
   - Read and check all the acknowledgment boxes
   - Click **Submit**

3. **Authenticate with GitHub**
   - You'll see a device code (e.g., `ABCD-1234`)
   - Click the link or go to: [github.com/login/device](https://github.com/login/device)
   - Sign in to GitHub if needed
   - Enter the device code
   - Click **Authorize HACS**

4. **Complete setup**
   - Back in Home Assistant, the setup will complete automatically
   - Assign HACS to an area if desired
   - Click **Finish**

HACS is now installed. You'll see it in the sidebar menu.

---

## Next Steps

Continue to **[Exergy HA Integrations](../integrations/overview.md)** to connect your bitcoin miners.
