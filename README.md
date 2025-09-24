# Advanced Derivatives & Option Pricing (Python)

A hands-on notebook exploring Black–Scholes pricing, implied volatility, and Monte Carlo valuation of path-dependent options (e.g., Asian and gap options) — with convergence checks to see what actually works.

## What’s inside
- **Closed-form Black–Scholes:** calls/puts, put–call parity, optional dividend yield `q`.
- **Implied volatility (IV):** Brent root-finder to invert Black–Scholes from price → σ.
- **Risk-neutral Monte Carlo:** GBM simulation, discounting expected payoffs at `r`.
- **Path-dependent payoffs:** arithmetic-average **Asian** option and **Gap** option (separate trigger & payoff strikes).
- **Convergence & diagnostics:** error vs √N, time-step effects, reproducible seeds.

> Example sanity check: recovering a synthetic price with σₜᵣᵤₑ = 0.25 gives IV ≈ **0.2500** (absolute error < 1e−4). Results vary with parameters/seed.

## Quickstart
1. **Clone the repo**
   ```bash
   git clone <your-repo-url>
   cd <your-repo>
2. Install dependencies (minimum):
   ```bash
   pip install numpy scipy matplotlib pandas
3. Open **Advanced-Porfolio-Optimisation.ipynb** in Jupyter or VS Code and **Run All**.

## How it works (short version)

- **Black–Scholes engine:** analytic prices for calls/puts with inputs `(S, K, r, σ, T, q)`.
- **IV solver:** solve `BS_price(σ) − observed_price = 0` with `scipy.optimize.brentq`.
- **Risk-neutral MC:** simulate under `dS = (r − q)S dt + σ S dW`; discount payoff by `exp(−rT)`.
- **Asian option:** arithmetic average of the path; show MC **convergence** vs paths/steps.
- **Gap option:** trigger strike vs payoff strike to illustrate discontinuous payoffs.

## Config you’ll likely touch

- **Market/contract:** `S0`, `K`, `r`, `T`, `sigma`, optional `q`.
- **Payoff:** `option='call'|'put'`, `asian=True|False`, gap parameters (`K_trigger`, `K_payoff`).
- **Monte Carlo:** `n_paths`, `n_steps`, `seed`.
- **Numerics:** IV tolerance/bounds, plot sample sizes for convergence.

## Results you can expect

- **Analytic vs simulated** price comparisons where closed forms exist.
- **Implied vol** that matches synthetic ground truth closely.
- **Convergence plots** showing MC error shrinking ≈ `1/√n_paths`.
- Tables/prints with price, standard error, and run settings for reproducibility.

## Notes & caveats

- MC noise & discretisation bias are real — increase paths/steps for tougher payoffs.
- Barrier-like features need dense timestepping or Brownian-bridge fixes (not included).
- Greeks (if computed) are bump-and-revalue and approximate.
- Educational project — **not investment advice**.

## Roadmap / nice-to-haves

- Variance reduction: antithetic & control variates.
- Longstaff–Schwartz for American/early-exercise features.
- Stochastic volatility (Heston) and/or stochastic rates.
- Pathwise/adjoint Greeks; auto-diff.
- Calibration examples using real option chains.
