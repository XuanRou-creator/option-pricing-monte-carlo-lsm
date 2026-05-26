# option-pricing-monte-carlo-ls# Option Pricing — Monte Carlo & LSM

Monte Carlo simulation methods for pricing exotic and path-dependent options, with variance reduction techniques and the Longstaff-Schwartz algorithm for American options.

---

## Projects Overview

| Notebook | Option Type | Method |
|----------|-------------|--------|
| `HW2_barrier_option_variance_reduction` | Up-and-Out Call | Standard MC · Antithetic Variates · Control Variate |
| `HW3_Q3_asian_call_control_variate` | Arithmetic Asian Call | Monte Carlo + Geometric Asian Control Variate |
| `HW3_Q4_american_put_lsm` | American Put | Longstaff-Schwartz (LSM) |

---

## 1. Barrier Option Pricing with Variance Reduction

Pricing an **up-and-out call** — a barrier option that expires worthless if the underlying price touches barrier level L before maturity.

Four methods implemented and compared under the same parameters (S₀=10, K=10, L=12, r=4%, σ=40%, T=1/12):

| Method | Standard Error | CI Width |
|--------|---------------|----------|
| Standard Monte Carlo | 0.001454 | 0.005700 |
| Antithetic Variates | 0.001156 | 0.004532 |
| European Call Control Variate | 0.001289 | 0.005052 |
| Modified Control Variate | 0.000837 | 0.003280 |

**Key finding:** Modified control variate achieves the smallest confidence interval (θ = 0.725), because its payoff structure `(S_T − K) · 1{K < S_T < L}` closely mirrors the up-and-out call payoff — much more so than a plain European call (θ = 0.291).

**Sensitivity analysis:** Results across barrier levels L = 11, 12, 13 and simulation counts N = 10,000 / 50,000 / 100,000, confirming SE ∝ 1/√N convergence.

---

## 2. Asian Call Option — Control Variate Method

Pricing an **arithmetic average Asian call** whose payoff depends on the average stock price over the path, rather than just the terminal price.

Since no closed-form solution exists for the arithmetic average, Monte Carlo is used with a **geometric average Asian call** as the control variate — chosen because:
- Its payoff is highly correlated with the arithmetic average payoff
- It has a known closed-form price

| Method | Price | Standard Deviation | Standard Error |
|--------|-------|--------------------|----------------|
| Ordinary Monte Carlo | 0.2731 | 0.4127 | 0.001305 |
| Control Variate | 0.2736 | 0.0062 | 0.0000197 |

**Key finding:** Control variate reduces standard deviation by ~99% (0.4127 → 0.0062), with θ* ≈ 1.018 indicating near one-to-one correlation between arithmetic and geometric payoffs.

---

## 3. American Put Option — Longstaff-Schwartz (LSM)

Pricing an **American put** using the Least-Squares Monte Carlo method, which solves the early exercise decision through backward induction.

At each time step, the algorithm compares:
- **Exercise value:** max(K − S_t, 0)
- **Continuation value:** estimated via OLS regression on in-the-money paths using basis {1, S_t, S_t²}

Results tested across n_paths = 10K / 50K / 100K and n_steps = 50 / 100 / 252 / 500:

| n_paths | n_steps | Price | Standard Error |
|---------|---------|-------|----------------|
| 100,000 | 50 | 0.4432 | 0.00166 |
| 100,000 | 252 | 0.4431 | 0.00164 |
| 100,000 | 500 | 0.4436 | 0.00161 |

**Key finding:** LSM converges stably across time step settings. Early exercise ratio ~30–40%, consistent with American put behavior. Standard error scales as 1/√N as expected.

---

## Parameters (Shared Across All Problems)

```python
S0    = 10       # Initial stock price
K     = 10       # Strike price
r     = 0.04     # Risk-free rate
sigma = 0.40     # Volatility
T     = 1/12     # Time to maturity (1 month)
N     = 100000   # Number of simulation paths
```

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python |
| Simulation | NumPy |
| Data Processing | Pandas |
| Visualization | Matplotlib |
| Environment | Google Colab |

---

## Project Structure

```
option-pricing-monte-carlo-lsm/
├── README.md
├── HW2_barrier_option_variance_reduction.ipynb
├── HW3_Q3_asian_call_control_variate.ipynb
├── HW3_Q4_american_put_lsm.ipynb
└── report/
    ├── HW2.pdf
    └── HW3.pdf
```

---

*Completed as part of Financial Engineering coursework at National Tsing Hua University (NTHU).*
