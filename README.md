# 🏡 Rent vs. Buy Pro: Interactive Financial Simulator

**Rent vs. Buy Pro** is a single-file financial modeling web application that evaluates the true long-term wealth impact of buying a home versus renting and investing. There is no build step and nothing to install — just open `index.html`.

Unlike basic mortgage calculators that only compare monthly rent against monthly Principal & Interest (P&I), this engine models **monthly amortization schedules**, **incremental tax shields (Standard Deduction hurdles & SALT caps)**, **unrecoverable sunk costs**, **Private Mortgage Insurance (PMI)**, **realtor liquidation fees**, and the **compounded opportunity cost of capital** over a customizable 1-to-30-year timeline.

---

## ✨ Key Features

* **📈 Dual-Portfolio Wealth Engine:** Tracks both the Buyer's net liquidated home equity (plus any side investment portfolio if renting becomes more expensive than owning in later years) and the Renter's compounded investment portfolio.
* **🧾 Realistic Tax Deduction Modeling:** Accurately models the U.S. tax code hurdle—comparing itemized mortgage interest (capped at $750k loan principal) and property taxes (subject to configurable SALT caps of $10k, $20k, or uncapped) against the Standard Deduction ($15,000 Single / $30,000 Married Filing Jointly).
* **🛡️ Automatic PMI Calculation & Drop-off:** Automatically applies Private Mortgage Insurance ($0.65\%$ annually) when the down payment is below $20\%$ and dynamically removes it the exact month the loan-to-value (LTV) reaches $80\%$.
* **🔥 Unrecoverable ("Sunk") Cost Tracking:** Separates equity-building principal payments from unrecoverable expenses (mortgage interest, property taxes, HOA/condo fees, maintenance, homeowners insurance, and buying/selling closing costs minus tax savings).
* **🎯 Exact Breakeven Horizon Detection:** Calculates the exact fractional year and month when buying surpasses renting in net liquidated wealth.
* **📊 Interactive HTML5 Canvas Visualizations:**
  * **Net Wealth Trajectory Chart:** Scrub across years to compare Buyer vs. Renter net worth.
  * **Cumulative Sunk Costs Chart:** Compare total unrecoverable money spent over time.
  * **Cash Outflow vs. Retained Equity Bar:** Visual breakdown of every dollar spent across categories.
* **📑 Expandable Amortization & Wealth Ledger:** Year-by-year table with clickable rows that expand to reveal granular principal, interest, tax shields, HOA/maintenance, and estimated selling fees, complete with one-click **CSV Export**. Switch the table between annual totals and monthly averages.
* **🔍 Monthly Cash-Flow Inspector:** Compares the buyer's and renter's monthly outlay side by side — for Year 1, any chosen year, or a full-timeline monthly average — with a per-line breakdown (P&I, property tax, HOA, maintenance, insurance, PMI and tax shield versus rent).
* **💸 After-Tax Liquidation:** Both positions are liquidated at the horizon. Home appreciation above the IRS Section 121 exclusion ($250k Single / $500k Married) and investment gains above cost basis are taxed at a configurable long-term capital gains rate, so the verdict reflects money you could actually keep.
* **⚡ Instant Scenario Presets & Dark/Light Mode:** Switch between *Baseline 10-Yr*, *High-Rate / Urban Condo*, *Suburban Family Home*, and *Aggressive Investor* presets, or toggle between Dark and Light themes.

---

## 🧮 Financial & Mathematical Methodology

The simulator operates on a **monthly compounding granularity** ($m = 1, 2, \dots, 12 \times N$) across an analysis horizon of $N$ years.

### 1. Mortgage Amortization (Monthly P&I)
Given a home purchase price $P_0$ and down payment percentage $d$, the initial loan principal is $L_0 = P_0 \times (1 - d)$. For an annual nominal mortgage interest rate $r_m$ and loan term of $T$ years ($M = 12 \times T$ total months), the monthly interest rate is $i_m = \frac{r_m}{12}$, and the fixed monthly Principal & Interest payment ($\text{PI}$) is:

$$\text{PI} = L_0 \cdot \frac{i_m (1 + i_m)^M}{(1 + i_m)^M - 1}$$

For each month $t$, interest and principal portions are:

$$\text{Interest}_t = L_{t-1} \cdot i_m, \quad \text{Principal}_t = \min\left(L_{t-1}, \text{PI} - \text{Interest}_t\right), \quad L_t = L_{t-1} - \text{Principal}_t$$

### 2. Home Appreciation & Ongoing Ownership Costs
Home value compounds monthly at an effective rate derived from the annual appreciation rate $g_h$:

$$P_t = P_{t-1} \cdot (1 + g_h)^{1/12}$$

* **Property Taxes & Maintenance:** Scale dynamically with the appreciating home value $P_t$.
* **HOA Fees & Homeowners Insurance:** Scale annually with general inflation ($2.5\%$ per year).
* **PMI:** Applied in month $t$ if $L_t > 0.80 \cdot P_0$.

### 3. Incremental Tax Shield (Standard Deduction Hurdle & SALT Cap)
Many calculators overestimate tax benefits by assuming every dollar of mortgage interest is deductible. In reality, homeownership only yields a tax benefit to the extent that **total itemized deductions exceed the Standard Deduction** ($\text{SD}$) you would have claimed anyway as a renter.

Let $S_{\text{other}}$ be existing state/local income taxes paid, $S_{\text{cap}}$ be the SALT cap limit, $T_{\text{prop}, y}$ be annual property taxes paid in year $y$, and $I_y$ be annual mortgage interest paid. The deductible mortgage interest is pro-rated if $L_0 > \$750,000$:

$$I_{\text{ded}, y} = I_y \cdot \min\left(1, \frac{750,000}{L_0}\right)$$

The Renter's baseline deduction and Buyer's total deduction in year $y$ are:

$$D_{\text{renter}} = \max\left(\text{SD}, \min(S_{\text{cap}}, S_{\text{other}})\right)$$

$$D_{\text{buyer}, y} = \max\left(\text{SD}, \min(S_{\text{cap}}, S_{\text{other}} + T_{\text{prop}, y}) + I_{\text{ded}, y}\right)$$

With marginal income tax rate $\tau_{\text{inc}}$, the net annual tax shield is:

$$\text{TaxShield}_y = \max\left(0, D_{\text{buyer}, y} - D_{\text{renter}}\right) \cdot \tau_{\text{inc}}$$

### 4. Opportunity Cost & Monthly Cash-Flow Differential Investing
To ensure an apples-to-apples comparison, both parties must allocate the **exact same total cash outflow** each month:
1. **Day 0 Upfront Capital:** The Buyer spends the Down Payment plus Buying Closing Costs ($C_{\text{buy}} = P_0 \cdot c_{\text{buy}}$). The Renter invests this exact upfront sum ($W_{\text{rent}, 0} = d \cdot P_0 + C_{\text{buy}}$) into their investment portfolio on Day 0.
2. **Monthly Differential Investing:** In every month $t$, let $\text{Outflow}_{\text{buy}, t}$ be the Buyer's total net monthly cash outlay (P&I + Property Tax + HOA + Maintenance + Insurance + PMI $-$ Monthly Tax Shield) and $\text{Outflow}_{\text{rent}, t}$ be the Renter's monthly outlay (Rent + Renter's Insurance).
   * If $\text{Outflow}_{\text{buy}, t} > \text{Outflow}_{\text{rent}, t}$, the Renter invests the difference $(\text{Outflow}_{\text{buy}, t} - \text{Outflow}_{\text{rent}, t})$ into their brokerage portfolio.
   * If $\text{Outflow}_{\text{rent}, t} > \text{Outflow}_{\text{buy}, t}$, the Buyer invests the savings $(\text{Outflow}_{\text{rent}, t} - \text{Outflow}_{\text{buy}, t})$ into a side brokerage portfolio.
3. Both portfolios compound monthly at $(1 + r_{\text{inv}})^{1/12} - 1$.

### 5. Liquidation & Net Wealth at Horizon Year $N$
At the end of year $y$, both positions are evaluated on an after-tax, post-liquidation basis:
* **Buyer Net Wealth:** Home value minus remaining loan balance $L_y$, minus Realtor selling fees ($P_y \cdot c_{\text{sell}}$), minus any capital gains tax on home appreciation above the IRS Section 121 exclusion ($\$250,000$ Single / $\$500,000$ Married), plus the Buyer's after-tax side investment portfolio.
* **Renter Net Wealth:** Total compounded portfolio value minus long-term capital gains tax ($\tau_{\text{cg}}$) applied only to the net investment gains above total contributed cost basis.

---

## 🚀 Usage & Deployment Instructions

### Running Locally
Because the project is built as a self-contained single-file architecture, no Node.js installation, bundler, or build step is required.

1. Clone the repository:
   ```bash
   git clone https://github.com/fwaadahmad1/rent-vs-buy-simulator.git
   cd rent-vs-buy-simulator
   ```
2. Open `index.html` directly in any modern web browser:
   * **macOS:** `open index.html`
   * **Windows:** `start index.html`
   * **Linux:** `xdg-open index.html`

> Styling and icons are loaded from CDNs (Tailwind, Phosphor Icons, Google Fonts), so an internet connection is needed for the page to render as intended.

### Hosting on GitHub Pages
1. Push `index.html` and `README.md` to your GitHub repository's `main` branch.
2. Navigate to your repository on GitHub and click **Settings** $\rightarrow$ **Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
4. Select the `main` branch and `/ (root)` directory, then click **Save**.
5. Within 60 seconds, your simulator will be live at `https://fwaadahmad1.github.io/rent-vs-buy-simulator/`.

---

## 🛠️ Tech Stack

* **Markup & Styling:** HTML5, [Tailwind CSS (CDN)](https://tailwindcss.com/) with dark/light mode support
* **Typography:** `Inter` (UI) & `JetBrains Mono` (tabular financial figures) via Google Fonts
* **Icons:** [Phosphor Icons](https://phosphoricons.com/)
* **Visualizations:** Vanilla JavaScript & high-DPI HTML5 `<canvas>` rendering (zero external charting library bloat)

---

## 📄 License

Distributed under the [MIT License](LICENSE). Feel free to fork, customize, and adapt for personal or educational financial planning.