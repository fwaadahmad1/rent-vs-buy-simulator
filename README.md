# Rent vs. Buy Financial Simulator

Single-file, client-side simulator that models the wealth and cash-flow outcome of renting versus buying over a 1–30 year horizon.

Open `index.html` in a browser — no build step, no server, no data leaves the page.

## What it models

- **Mortgage**: fixed-rate amortization, loan term, down payment, PMI (0.65%/yr until 80% LTV), buyer/seller closing costs.
- **Ownership costs**: property tax, HOA, maintenance (% of home value), homeowner's insurance, inflated at 2.5%/yr where applicable.
- **Taxes**: itemized vs. standard deduction (single/married/none), SALT cap, $750k mortgage-interest deduction cap (pro-rated), $250k/$500k home-sale capital-gains exclusion, capital-gains tax on liquidated portfolios.
- **Opportunity cost**: renter invests the upfront cash (down payment + closing costs) instead; whichever side spends less each month invests the difference.
- **Outputs**: year-by-year net wealth for both paths, dollar breakeven year, sunk-cost breakdown, and dual-line charts.

## Presets

Baseline 10-Yr, High-Cost Condo, Suburban Family, High-Rate / Market Bull.

## Caveats

Simplified model, not financial advice: no local market data, taxes use a flat marginal rate, and inflation is fixed at 2.5%.
