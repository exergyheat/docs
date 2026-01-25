# Solar Monetization Calculator

The Solar Monetization Calculator helps you determine whether Bitcoin mining is a better use of your solar energy than selling it back to the grid. By comparing mining revenue against net metering compensation, you can make an informed decision about how to maximize the value of your solar production.

**[Open the Calculator](https://calc.exergyheat.com/#/solar)**

---

## Quick Start

1. **Choose your input mode** - Select how you want to provide solar production data
2. **Enter your solar details** - ZIP code and system size, or your own production numbers
3. **Select a miner** - Pick from presets or enter custom efficiency
4. **Compare against net metering** - See how mining stacks up against your utility's compensation
5. **Review your results** - See annual revenue, per-kWh earnings, and recommendations

The calculator uses live Bitcoin network data and updates results in real-time as you adjust inputs.

---

## Understanding Input Modes

The calculator offers three ways to model your solar production:

### Mode 1: Estimate (Location-Based)

Use this mode if you're planning a new solar installation or want a quick estimate.

| Input | Description |
|-------|-------------|
| **ZIP Code** | Your location for solar irradiance data |
| **System Size (kW)** | DC capacity of your solar array |

The calculator uses the **NREL PVWatts API** to estimate annual solar production based on:
- Local solar irradiance (kWh/m²/day)
- Typical system losses (14% default)
- Fixed south-facing tilt (latitude-optimal)

**Example:** A 10 kW system in Phoenix, AZ might produce ~18,000 kWh/year, while the same system in Seattle, WA might produce ~12,000 kWh/year.

### Mode 2: Production (User-Provided Generation)

Use this mode if you know your actual or expected solar production.

| Input | Description |
|-------|-------------|
| **Annual Production** | Total kWh your system generates per year |
| **Monthly Breakdown** (optional) | Production by month for seasonal analysis |

**When to use this mode:**
- You have an existing system with historical data
- You have a quote with production estimates
- You want to model a specific scenario

### Mode 3: Excess (Net Metering Comparison)

Use this mode to specifically compare mining your excess solar against net metering.

| Input | Description |
|-------|-------------|
| **Excess Solar** | kWh you export to the grid (don't self-consume) |
| **Net Metering Type** | How your utility compensates exported solar |
| **Compensation Rate** | What you receive per kWh exported |

This mode is ideal if you already offset your own usage and want to optimize what happens to the surplus.

---

## Net Metering Comparison

### Understanding Utility Compensation Types

Utilities compensate exported solar differently. The calculator models three common structures:

#### Bill Credits (Full Retail Rate)

**How it works:**
- Each kWh exported earns a credit equal to your retail electricity rate
- Credits offset future electricity purchases
- Unused credits typically expire after 12 months

**Typical rate:** ~$0.12/kWh (varies by location)

**Where it's common:** California (legacy NEM 2.0), many Northeast states, Colorado

**Pros:** Highest compensation rate
**Cons:** Credits can expire; no cash value

#### Net Billing (Avoided Cost Rate)

**How it works:**
- Each kWh exported is credited at a lower "avoided cost" or wholesale rate
- Credits are applied instantly to your bill
- Typically 30-50% of retail rate

**Typical rate:** ~$0.05/kWh (varies widely)

**Where it's common:** California NEM 3.0, Arizona, Idaho, Nevada

**Pros:** Credits don't expire
**Cons:** Much lower compensation than retail

#### Annual Cash-Out

**How it works:**
- kWh credits accumulate throughout the year
- Annual excess is paid out at a low wholesale rate
- Monthly credits may be at higher rate

**Typical rate:** ~$0.02-0.04/kWh for annual payout

**Where it's common:** Xcel Energy territories, TVA utilities, some rural co-ops

**Pros:** Receive actual cash for excess
**Cons:** Very low compensation rate

### Comparison Table

| Type | Typical Rate | Annual Value (5,000 kWh) |
|------|--------------|--------------------------|
| Bill Credits | $0.12/kWh | $600 in credits |
| Net Billing | $0.05/kWh | $250 in credits |
| Annual Cash-Out | $0.02/kWh | $100 in cash |

---

## Understanding the Calculations

### Core Mining Formula (kWh-Based)

The key insight: **Only miner efficiency matters for solar mining economics**—not power draw or hashrate individually.

```
Sats per kWh = (1000 / Efficiency_J_TH) × (Hashvalue / 24)
```

Where:
- **1000** converts kWh to Wh
- **Efficiency_J_TH** is miner efficiency in Joules per Terahash
- **Hashvalue** is sats earned per TH per day
- **24** converts daily hashvalue to hourly

**Why this works:**

A miner's hashrate is directly proportional to its power consumption:
```
Hashrate (TH/s) = Power (W) / Efficiency (J/TH)
```

So for any amount of energy (kWh), the BTC earned depends only on efficiency:
```
Total Sats = kWh × 1000 × (1 / Efficiency) × (Hashvalue / 24)
```

**Example:**
- Miner efficiency: 20 J/TH
- Hashvalue: 60 sats/TH/day
- Solar production: 1 kWh

```
Sats = 1 × 1000 × (1/20) × (60/24)
     = 1000 × 0.05 × 2.5
     = 125 sats per kWh
```

### Daily/Monthly/Annual Revenue

```
Daily BTC = Daily kWh × Sats per kWh / 100,000,000

Monthly BTC = Monthly kWh × Sats per kWh / 100,000,000

Annual BTC = Annual kWh × Sats per kWh / 100,000,000

USD Value = BTC × BTC Price
```

### Revenue per kWh

```
$/kWh (mining) = Sats per kWh × BTC Price / 100,000,000
```

**Example:** At 125 sats/kWh and $100,000 BTC:
```
$/kWh = 125 × 100,000 / 100,000,000 = $0.125/kWh
```

### Net Metering vs Mining Comparison

```
Mining Advantage = Mining $/kWh - Net Metering $/kWh

Recommendation:
- If Mining $/kWh > Net Metering $/kWh → "Mine your excess solar"
- If Mining $/kWh < Net Metering $/kWh → "Keep net metering"
- If close (within 20%) → "Consider mining for BTC accumulation"
```

---

## Bitcoin Network Data & Overrides

### Live Data Sources

| Metric | Source | Description |
|--------|--------|-------------|
| **BTC Price** | CoinGecko | Current Bitcoin price in USD |
| **Network Hashrate** | Mempool.space | Total computing power securing the network (EH/s) |
| **Hashprice** | Calculated | Revenue per TH/day in dollars |
| **Hashvalue** | Calculated | Revenue per TH/day in satoshis |

### The Three-Knob Override System

For "what-if" scenario analysis, you can override live data using three independent controls:

**Knob 1 - Price Group:**
- Edit **BTC Price** and hashprice auto-calculates, OR
- Edit **Hashprice** and BTC price auto-calculates

**Knob 2 - Network Group:**
- **Fee %** anchors this group
- Edit **Network Hashrate** and hashvalue recalculates, OR
- Edit **Hashvalue** and network hashrate recalculates

**Knob 3 - Fee Percentage:**
- Slider from **0-99%**
- Models transaction fee environments
- When adjusted, hashvalue recalculates (network hashrate held constant)

Click "Reset to live data" to clear overrides.

---

## Understanding the Results

### Primary Metrics

| Metric | What It Means |
|--------|---------------|
| **Annual BTC** | Total Bitcoin earned from mining your solar production |
| **Annual USD** | Dollar value of mining revenue at current BTC price |
| **Sats per kWh** | Satoshis earned per kilowatt-hour of solar used |
| **$/kWh (Mining)** | Dollar value earned per kWh when mining |

### Comparison Metrics (Excess Mode)

| Metric | What It Means |
|--------|---------------|
| **$/kWh (Net Metering)** | What your utility pays per kWh exported |
| **Mining Advantage** | Difference between mining and net metering $/kWh |
| **Annual Difference** | Total dollar difference over a year |
| **Recommendation** | Whether to mine or stick with net metering |

### Monthly Breakdown

The calculator shows month-by-month projections:
- Solar production (kWh)
- BTC earned
- USD value
- Comparison to net metering

This helps visualize seasonal variations—summer months typically produce more solar and thus more mining revenue.

---

## Miner Selection

For solar mining, **efficiency (J/TH) is the only spec that matters**. The calculator includes presets:

| Miner | Efficiency (J/TH) | Category |
|-------|-------------------|----------|
| **Avalon Mini 3** | 21.3 | Space heater class |
| **Avalon Q** | 18.9 | Space heater class |
| **Whatsminer M64** | 21.9 | HVAC class |
| **Bitmain S19k Pro** | 23.0 | HVAC class |
| **Bitmain S21** | 17.5 | Latest generation |
| **Antminer S9** | 103.7 | Legacy/budget |

**Custom input:** Enter any J/TH efficiency value directly.

> **Note:** Lower J/TH = more efficient = more sats per kWh. A 20 J/TH miner earns ~5x more per kWh than a 100 J/TH miner.

---

## Use Cases & Examples

### Example 1: Should I Mine My Excess Solar?

**Scenario:** You have a 10 kW system producing 14,000 kWh/year. After self-consumption, 5,000 kWh goes back to the grid. Your utility offers net billing at $0.045/kWh.

**Steps:**
1. Select "Excess" input mode
2. Enter 5,000 kWh excess
3. Select "Net Billing" at $0.045/kWh
4. Choose a miner (e.g., Avalon Mini 3 at 21.3 J/TH)
5. Review comparison

**Possible result:**
- Net metering value: 5,000 × $0.045 = $225/year
- Mining revenue: ~$400/year (at current network conditions)
- Recommendation: Mine your excess solar

### Example 2: New Solar Installation Planning

**Scenario:** You're installing a 12 kW system and want to know if you should oversize for mining.

**Steps:**
1. Select "Estimate" mode
2. Enter your ZIP code
3. Enter 12 kW system size
4. Compare mining revenue to net metering options
5. Model larger systems (15 kW, 20 kW) to see diminishing returns

### Example 3: What BTC Price Makes Mining Beat Net Metering?

**Scenario:** Your utility offers generous $0.10/kWh bill credits. At what BTC price does mining become better?

**Steps:**
1. Enter your solar production
2. Set net metering to $0.10/kWh
3. Use Knob 1 to adjust BTC price
4. Find the price where Mining $/kWh exceeds $0.10

---

## Key Relationships & Sensitivity

### What Drives Solar Mining Economics?

In order of impact:

1. **Miner efficiency (J/TH)** — The single most important variable. Upgrading from 100 J/TH to 20 J/TH increases revenue 5x.

2. **Bitcoin price** — Directly scales USD revenue. When BTC doubles, dollar revenue doubles.

3. **Net metering rate** — The lower your utility pays, the more attractive mining becomes.

4. **Network hashrate** — Inversely affects earnings. Higher network hashrate = less BTC per TH.

5. **Fee %** — Higher transaction fees increase miner revenue (when network is congested).

### Important Considerations

**Net metering is risk-free:**
Net metering compensation is guaranteed (at current rates). Mining revenue varies with BTC price and network conditions.

**Mining accumulates BTC:**
Even if current USD value is similar, mining lets you accumulate Bitcoin—potentially valuable if price appreciates.

**Utility rate changes:**
Net metering policies are changing rapidly. Many utilities are reducing compensation (NEM 3.0 trend). Mining provides optionality.

**Miner efficiency improvements:**
New miner generations improve efficiency ~20-30% every 1-2 years. Future miners will earn more per kWh.

---

## Technical Notes

### Data Sources

| Data | Source | Update Frequency |
|------|--------|-----------------|
| BTC Price | CoinGecko API | On page load |
| Network Hashrate | Mempool.space API | On page load |
| Solar Estimates | NREL PVWatts API | On ZIP code entry |
| Block Reward | Hardcoded | Updates at halvings |

### PVWatts API Parameters

When using Estimate mode, the calculator queries NREL PVWatts with:
- System capacity: User-provided kW
- Module type: Standard (crystalline silicon)
- Array type: Fixed (roof mount)
- Tilt: Location latitude
- Azimuth: 180° (south-facing)
- System losses: 14%

### Fallback Values

If APIs are unavailable:

| Metric | Fallback Value |
|--------|----------------|
| BTC Price | $100,000 USD |
| Network Hashrate | 800 EH/s |
| Block Reward | 3.125 BTC |

---

## Glossary

| Term | Definition |
|------|------------|
| **Avoided Cost** | The cost a utility avoids by not generating or purchasing power. Basis for net billing rates. |
| **Bill Credits** | kWh credits that offset future electricity purchases, typically at retail rate. |
| **Excess Solar** | Solar production exported to the grid (not self-consumed). |
| **Fee %** | Transaction fees as a percentage of total block reward. |
| **Hashvalue** | Satoshis earned per terahash per day. |
| **J/TH** | Joules per terahash. Measures miner efficiency. Lower = better. |
| **Net Billing** | Compensation for exported solar at avoided cost rate (lower than retail). |
| **Net Metering** | Utility programs that credit solar owners for exported electricity. |
| **NEM 3.0** | California's new net metering policy with reduced export compensation. |
| **NREL** | National Renewable Energy Laboratory. Provides PVWatts solar estimation API. |
| **PVWatts** | NREL tool for estimating solar production based on location and system size. |
| **Sats per kWh** | Satoshis earned per kilowatt-hour of energy used for mining. |
