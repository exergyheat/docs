# Hashrate Heating Calculator

The Hashrate Heating Calculator helps you determine whether Bitcoin mining is a cost-effective way to heat your home. By comparing the net cost of running a mining heater against traditional fuel sources, you can make an informed decision about whether hashrate heating makes sense for your situation.

**[Open the Calculator](https://calc.exergyheat.com)**

---

## Quick Start

1. **Select your location** - Choose your country and state/province to auto-load regional electricity and fuel prices
2. **Enter your electricity rate** - Use the direct input or bill calculator
3. **Choose a fuel to compare against** - Select what you currently use for heating
4. **Select a miner** - Pick from presets or enter custom specs
5. **Review your results** - See COPe, subsidy %, and savings vs. your current fuel

The calculator uses live Bitcoin network data and updates results in real-time as you adjust inputs.

---

## Understanding the Inputs

### Bitcoin Network Data

The calculator fetches live data from public APIs:

| Metric | Source | Description |
|--------|--------|-------------|
| **BTC Price** | CoinGecko | Current Bitcoin price in USD (converted to CAD for Canada) |
| **Network Hashrate** | Mempool.space | Total computing power securing the Bitcoin network (EH/s) |
| **Hashprice** | Calculated | Revenue earned per terahash per day ($/TH/day) |
| **Hashvalue** | Calculated | Satoshis earned per terahash per day (sats/TH/day) |

#### The Three-Knob Override System

For "what-if" scenario analysis, you can override live data using three independent controls:

**Knob 1 - Price Group:**
- Edit **BTC Price** and hashprice auto-calculates, OR
- Edit **Hashprice** and BTC price auto-calculates
- These two values are inversely implied—changing one recalculates the other

**Knob 2 - Network Group:**
- **Fee %** anchors this group
- Edit **Network Hashrate** and hashvalue recalculates (Fee % held constant), OR
- Edit **Hashvalue** and network hashrate recalculates (Fee % held constant)

**Knob 3 - Fee Percentage:**
- Slider from **0-99%**
- Models transaction fee environments
- When adjusted, hashvalue recalculates (network hashrate held constant)
- Higher Fee % = more miner revenue per block

**How Fee % Affects Calculations:**

The block reward miners compete for consists of two parts:
1. **Block subsidy** (currently 3.125 BTC)
2. **Transaction fees** (variable based on network demand)

Fee % represents transaction fees as a percentage of total block reward:
```
Total Block Reward = Block Subsidy / (1 - Fee %)
```

For example, at 20% fee environment:
```
Total Block Reward = 3.125 / (1 - 0.20) = 3.906 BTC
```

This means miners earn ~25% more than the base subsidy alone.

**Example Scenarios:**
- "What if BTC hits $150,000?" → Adjust BTC Price (Knob 1)
- "What happens when hashrate doubles?" → Adjust Network Hashrate (Knob 2)
- "What if transaction fees spike during an ordinals frenzy?" → Increase Fee % (Knob 3)

Click "Reset to live data" to clear all overrides.

---

### Location Selection

| Input | Options | Effect |
|-------|---------|--------|
| **Country** | United States, Canada | Sets currency, units, and available regions |
| **State/Province** | 50 US states, 13 Canadian provinces, or "National Average" | Auto-loads regional electricity and fuel prices |

Selecting a region automatically populates default electricity and fuel rates based on regional averages. You can override these with your actual rates.

**Unit differences by country:**

| Fuel | US Units | Canadian Units |
|------|----------|----------------|
| Natural Gas | $/therm | $/GJ |
| Propane | $/gallon | $/litre |
| Heating Oil | $/gallon | $/litre |
| Wood Pellets | $/bag | $/bag |

---

### Electricity Rate

Your electricity rate is the single most important input for hashrate heating economics.

**Direct entry:** Enter your rate in $/kWh (or C$/kWh for Canada)

**Bill calculator:** If you don't know your rate, enter:
- Total bill amount ($)
- kWh consumed

The calculator divides bill by kWh to determine your all-in rate (including delivery, taxes, and fees).

> **Tip:** Use your winter bill for the most accurate heating season rate. Time-of-use customers should use their off-peak rate if running miners during off-peak hours.

---

### Fuel Comparison

Select the fuel you want to compare hashrate heating against:

| Fuel Type | BTU per Unit (US) | BTU per Unit (CA) | Default Efficiency |
|-----------|-------------------|-------------------|-------------------|
| **Natural Gas** | 100,000/therm | 947,817/GJ | 92% AFUE |
| **Propane** | 91,500/gallon | 24,200/litre | 90% AFUE |
| **Heating Oil** | 138,500/gallon | 36,600/litre | 85% AFUE |
| **Electric Resistance** | 3,412/kWh | 3,412/kWh | 100% |
| **Heat Pump** | 3,412/kWh | 3,412/kWh | 300% (COP 3.0) |
| **Wood Pellets** | 330,000/bag | 330,000/bag | 80% |

**Inputs:**
- **Fuel rate** - Cost per unit (auto-populated from region, can override)
- **Efficiency** - AFUE percentage for combustion fuels, or COP for heat pumps

**Bill calculator:** Similar to electricity, enter total fuel cost and units consumed to calculate your actual rate.

---

### Miner Selection

Choose from 8 preset miners or enter custom specifications:

| Miner | Power (W) | Hashrate (TH/s) | Efficiency (J/TH) | Use Case |
|-------|-----------|-----------------|-------------------|----------|
| **Heatbit Trio** | 400 | 10 | 40.0 | Small space heater |
| **Heatbit Maxi** | 1,500 | 39 | 38.5 | Medium space heater |
| **Avalon Mini 3** | 850 | 40 | 21.3 | Compact space heater |
| **Avalon Q** | 1,700 | 90 | 18.9 | Large space heater |
| **Whatsminer M64** | 5,000 | 228 | 21.9 | HVAC integration |
| **Bitmain S19j Pro** | 3,068 | 104 | 29.5 | HVAC integration |
| **Bitmain S19k Pro** | 2,760 | 120 | 23.0 | HVAC integration |
| **Bitmain S9** | 1,400 | 13.5 | 103.7 | Legacy/budget option |

**Custom input:** Enter any power (W) and hashrate (TH/s) combination. The calculator auto-switches to "Custom" if you modify a preset's values.

> **Note:** All Bitcoin miners are 100% efficient as heaters—every watt of electricity becomes heat. The J/TH efficiency only affects mining revenue, not thermal output.

---

## Understanding the Calculations

### Core Formulas

#### Hashvalue (sats/TH/day)

The number of satoshis a single terahash of mining power earns per day:

```
Hashvalue = (Blocks per Day × Total Block Reward × sats per BTC) / Network Hashrate
```

Where Total Block Reward accounts for transaction fees:
```
Total Block Reward = Block Subsidy / (1 - Fee %)
                   = 3.125 / (1 - Fee %)
```

**At 0% fees (subsidy only):**
```
Hashvalue = (144 × 3.125 × 100,000,000) / Network Hashrate (TH/s)
         = 45,000,000,000 / Network Hashrate (TH/s)
```

**Example at 800 EH/s with 0% fees:**
```
Hashvalue = 45,000,000,000 / 800,000,000 = 56.25 sats/TH/day
```

**Example at 800 EH/s with 20% fees:**
```
Total Block Reward = 3.125 / 0.80 = 3.906 BTC
Hashvalue = (144 × 3.906 × 100,000,000) / 800,000,000 = 70.31 sats/TH/day
```

#### Hashprice ($/TH/day)

The USD value of hashvalue:

```
Hashprice = Hashvalue × BTC Price / 100,000,000
```

**Example:** At 56.25 sats/TH/day and $100,000 BTC:
```
Hashprice = 56.25 × 100,000 / 100,000,000 = $0.05625/TH/day
```

#### Daily Bitcoin Earnings

Your share of the daily block rewards:

```
Daily BTC = (Your Hashrate / Network Hashrate) × Blocks per Day × Block Reward

          = (Your Hashrate TH/s / Network Hashrate TH/s) × 144 × 3.125
```

**Example:** 50 TH/s miner at 800 EH/s network:
```
Daily BTC = (50 / 800,000,000) × 144 × 3.125
          = 0.0000000625 × 450
          = 0.0000281 BTC (2,810 sats)
```

#### Daily Electricity Cost

```
Daily kWh = (Miner Power in Watts / 1000) × 24 hours

Daily Electricity Cost = Daily kWh × Electricity Rate ($/kWh)
```

**Example:** 1,000W miner at $0.12/kWh:
```
Daily kWh = (1000 / 1000) × 24 = 24 kWh
Daily Cost = 24 × $0.12 = $2.88
```

#### Daily Mining Revenue

```
Daily Revenue = Daily BTC × BTC Price
```

**Example:** 0.0000281 BTC at $100,000:
```
Daily Revenue = 0.0000281 × $100,000 = $2.81
```

---

### Key Economic Metrics

#### Revenue Ratio (R) — "Heating Subsidy"

The percentage of electricity cost offset by mining revenue:

```
R = Daily Mining Revenue / Daily Electricity Cost
```

**Example:** $2.81 revenue / $2.88 electricity cost:
```
R = 2.81 / 2.88 = 0.976 (97.6%)
```

**Interpretation:**
- R = 0%: No mining revenue (broken miner or offline)
- R = 50%: Mining covers half your electricity cost
- R = 100%: Mining covers all electricity cost (free heating)
- R > 100%: Mining exceeds electricity cost (you profit while heating)

#### COPe (Coefficient of Performance - Economic)

A metric analogous to heat pump COP, measuring effective heating efficiency:

```
COPe = 1 / (1 - R)
```

| R (Subsidy) | COPe | Interpretation |
|-------------|------|----------------|
| 0% | 1.0 | Same as electric resistance heating |
| 50% | 2.0 | Equivalent to a basic heat pump |
| 75% | 4.0 | Equivalent to a high-efficiency heat pump |
| 90% | 10.0 | Extremely efficient |
| 100% | ∞ | Free heating |
| >100% | Negative | You're being paid to heat |

**Why COPe matters:** It lets you directly compare hashrate heating to heat pumps. If your COPe is 3.0 and your heat pump's COP is 3.0, they cost the same to operate. If COPe > COP, the miner is cheaper.

#### Effective Cost per kWh

The net cost of heat after mining revenue offset:

```
Effective $/kWh = (Daily Electricity Cost - Daily Mining Revenue) / Daily kWh
```

**Example:** ($2.88 - $2.81) / 24 kWh:
```
Effective $/kWh = $0.07 / 24 = $0.0029/kWh
```

This can also be expressed in other units:
```
Effective $/therm = Effective $/kWh × 29.307 kWh/therm

Effective $/MMBTU = Effective $/kWh × 293.07 kWh/MMBTU
```

#### Break-even Electricity Rate

The maximum electricity rate where R = 100% (free heating):

```
Break-even Rate = Daily Mining Revenue / Daily kWh

                = (Daily BTC × BTC Price) / Daily kWh
```

If your actual rate is below the break-even rate, you heat for free (or profit).

---

### Fuel Comparison Calculations

#### Traditional Fuel Cost per kWh

To compare against hashrate heating, we convert fuel costs to $/kWh equivalent:

```
Fuel $/kWh = (BTU per kWh / BTU per Fuel Unit) × Fuel Price × (1 / Efficiency)

           = (3,412 / Fuel BTU Content) × $/unit × (1 / Efficiency)
```

**Example:** Natural gas at $1.50/therm, 92% efficiency:
```
Fuel $/kWh = (3,412 / 100,000) × $1.50 × (1 / 0.92)
           = 0.03412 × $1.50 × 1.087
           = $0.0556/kWh
```

#### Savings Percentage

```
Savings % = (Fuel $/kWh - Hashrate $/kWh) / Fuel $/kWh × 100
```

**Example:** Fuel at $0.0556/kWh, hashrate at $0.0029/kWh:
```
Savings % = (0.0556 - 0.0029) / 0.0556 × 100 = 94.8%
```

---

## Understanding the Results

### Primary Metrics

| Metric | What It Means |
|--------|---------------|
| **COPe** | Economic efficiency compared to electric resistance (1.0). Higher = better. Compare to heat pump COP. |
| **Subsidy %** | How much of your electricity cost mining revenue covers. 100% = free heating. |
| **Savings vs [Fuel]** | Percentage savings compared to your selected fuel. Positive = hashrate heating is cheaper. |
| **Effective Cost** | Your net cost per unit of heat after mining revenue. Lower = better. |

### Secondary Metrics

| Metric | What It Means |
|--------|---------------|
| **Break-even Rate** | Maximum $/kWh for free heating. If your rate is lower, you profit. |
| **Daily/Monthly BTC** | Expected Bitcoin earnings at current network conditions. |
| **Monthly Sats** | Same as above, in satoshis. |

### Status Indicators

The calculator displays a status based on your results:

| Status | Condition | Meaning |
|--------|-----------|---------|
| **Profitable** | R ≥ 100% | Mining revenue exceeds electricity cost. You profit while heating. |
| **Subsidized** | Savings > 0% | Hashrate heating is cheaper than your alternative fuel. |
| **Loss** | Savings ≤ 0% | Your alternative fuel is currently cheaper. |

---

## Interactive Features

### Sensitivity Charts

Each major metric (Savings, COPe, Subsidy) has an expandable chart showing how results change when you vary different inputs.

**Available X-axis variables:**
- **Electricity rate** - See how rate changes affect your economics
- **Fuel rate** - See sensitivity to fuel price fluctuations
- **Miner efficiency** - Compare different miner classes (J/TH)
- **Hashprice** - Model different Bitcoin market conditions

**Reading the charts:**
- The **amber dot** marks your current value
- **Reference lines** show important thresholds (e.g., 100% subsidy, heat pump COP)
- Steeper curves indicate higher sensitivity to that variable

### Geographic Heat Map

The interactive map visualizes hashrate heating economics across all US states or Canadian provinces.

**Features:**
- **Color gradient** from red (poor economics) to green (excellent economics)
- **Three metric views:** Savings %, COPe, or Subsidy %
- **Click any region** to populate the calculator with that region's rates
- **Hover tooltip** shows region name, rates, and all three metrics
- **"YOU" row** compares your custom inputs against regional averages
- **Mobile-friendly table view** with sortable columns

**Using the map:**
1. Select which metric to display
2. For Savings %, also select the fuel type for comparison
3. Hover over regions to see details
4. Click a region to model that location

---

## Key Relationships & Sensitivity

### What Has the Strongest Influence?

In order of impact:

1. **Electricity rate** — The most sensitive variable. A $0.05/kWh difference can swing results from profitable to loss.

2. **Bitcoin price / Hashprice** — Directly scales mining revenue. When BTC price doubles, mining revenue doubles.

3. **Miner efficiency (J/TH)** — More efficient miners earn more per watt, improving economics. However, less efficient miners output more heat per dollar of hardware.

4. **Fuel rate and efficiency** — Only affects the comparison to traditional heating, not absolute hashrate heating costs.

5. **Network hashrate** — Inversely affects earnings. When network hashrate doubles, individual earnings halve. This is the variable most outside your control.

### Important Trade-offs

**Electricity rate vs. fuel rate:**
Even with expensive electricity, hashrate heating can win if your alternative fuel is also expensive (e.g., propane in rural areas).

**Miner efficiency vs. heat output:**
More efficient miners (lower J/TH) have better economics but often produce less heat. A 400W space heater won't warm a large room regardless of its COPe.

**BTC price volatility:**
Results are highly sensitive to BTC price. A 50% price drop roughly halves your subsidy percentage. Consider modeling worst-case scenarios.

**Network hashrate growth:**
Hashrate tends to increase over time, reducing per-TH earnings. This is partially offset by BTC price appreciation and block reward expectations.

---

## Use Cases & Examples

### Example 1: Am I Better Off With Hashrate Heating?

**Scenario:** You currently heat with propane at $2.80/gallon with a 90% efficient furnace. Your electricity rate is $0.14/kWh.

**Steps:**
1. Select your state (or enter rates manually)
2. Set electricity rate to $0.14/kWh
3. Select "Propane" as fuel type, enter $2.80/gallon, 90% efficiency
4. Choose a miner (e.g., Avalon Mini 3)

**Interpreting results:**
- If Savings shows **+45%**, hashrate heating costs 45% less than propane
- If COPe shows **3.2**, your miner is as efficient as a 3.2 COP heat pump
- If Subsidy shows **78%**, mining covers 78% of your electricity cost

### Example 2: What BTC Price Do I Need for Free Heating?

**Scenario:** You want to find the minimum BTC price for 100% subsidy (free heating).

**Steps:**
1. Enter your actual electricity rate
2. Select your miner
3. In the Bitcoin Data section, manually adjust the BTC Price override
4. Watch the Subsidy % metric
5. Find the price where Subsidy reaches ~100%

**Alternative method:**
- Note your **Break-even Rate** result
- If your actual rate is below this, you already have free heating
- If above, the ratio tells you how much BTC needs to rise

### Example 3: Which Region Is Best for Hashrate Heating?

**Scenario:** You're flexible on location and want to find the best state for hashrate heating.

**Steps:**
1. Set your preferred fuel type for comparison
2. Expand the Geographic Heat Map
3. Select "Savings %" as the metric
4. Look for the darkest green regions
5. Click promising states to see full details

**Best regions typically have:**
- Low electricity rates (< $0.10/kWh)
- High alternative fuel costs (expensive propane/oil)
- Cold winters (more heating demand)

---

## Technical Notes

### Data Sources

| Data | Source | Update Frequency |
|------|--------|-----------------|
| BTC Price | CoinGecko API | On page load |
| Network Hashrate | Mempool.space API | On page load |
| Regional Fuel Prices | Internal database | Periodic updates |
| Block Reward | Hardcoded | Updates at halvings (next: ~2028) |

### Fallback Values

If APIs are unavailable, the calculator uses these defaults:

| Metric | Fallback Value |
|--------|----------------|
| BTC Price | $100,000 USD |
| Network Hashrate | 800 EH/s |
| Block Reward | 3.125 BTC |
| Hashprice | $0.05/TH/day |

### Currency Conversion

For Canadian users:
- BTC price is converted at 1 USD = 1.40 CAD
- All results display in CAD
- Fuel units use metric (litres, GJ)

---

## Glossary

| Term | Definition |
|------|------------|
| **AFUE** | Annual Fuel Utilization Efficiency. Percentage of fuel converted to heat. |
| **Block Subsidy** | The fixed BTC reward per block (currently 3.125 BTC). Halves approximately every 4 years. |
| **COP** | Coefficient of Performance. Heat output divided by electricity input (for heat pumps). |
| **COPe** | Coefficient of Performance - Economic. Exergy's metric comparing hashrate heating to electric resistance. |
| **EH/s** | Exahashes per second. 1 EH = 1,000,000 TH. Measures Bitcoin network hashrate. |
| **Fee %** | Transaction fees as a percentage of total block reward. Models periods of high/low network demand. |
| **Hashprice** | Mining revenue per terahash per day, in dollars. |
| **Hashvalue** | Mining revenue per terahash per day, in satoshis. |
| **J/TH** | Joules per terahash. Measures miner efficiency. Lower = more efficient. |
| **R** | Revenue ratio. Mining revenue divided by electricity cost. Same as Subsidy %. |
| **TH/s** | Terahashes per second. Measures individual miner hashrate. |
| **Total Block Reward** | Block subsidy plus transaction fees. What miners actually earn per block. |
