# Portfolio-Risk-Analysis
This notebook implements a multi-factor Value-at-Risk (VaR) framework for a portfolio consisting of equity positions and an interest rate swap. Risk is decomposed using Principal Component Analysis (PCA) on the SOFR curve, and VaR is estimated under three methods: Parametric, Monte Carlo, and Historical Simulation. Results are backtested against the Basel III traffic-light framework.This notebook implements a multi-factor Value-at-Risk (VaR) framework for a portfolio consisting of equity positions and an interest rate swap. Risk is decomposed using Principal Component Analysis (PCA) on the SOFR curve, and VaR is estimated under three methods: Parametric, Monte Carlo, and Historical Simulation. Results are backtested against the Basel III traffic-light framework.
Methodology

1. SOFR Curve PCA

Daily absolute changes in the SOFR curve are decomposed into principal components. The first three PCs (Level, Slope, Curvature) are retained, which together explain the vast majority of curve variance. Factor scores are extracted as time series and used as the rate risk factors driving the swap's P&L.

2. Swap Pricing

The payer swap is priced by interpolating zero rates from the reconstructed SOFR curve and computing the present value of fixed and floating legs using continuous discounting. The current curve is reconstructed from the mean curve plus the PCA factor scores on the valuation date (2023-10-30).

3. Sensitivity Calculation

Finite-difference deltas of the swap with respect to each PC score are computed via central differencing. These sensitivities map PC shocks to swap P&L and are used in both the sensitivity-based and parametric VaR calculations.

4. VaR Estimation

Parametric VaR
Portfolio loss is modelled as a linear combination of risk factor returns (stock returns + PC changes). Loss distribution is assumed normal; VaR is computed analytically using the portfolio mean and variance.

Monte Carlo VaR
100,000 correlated scenarios are drawn using Cholesky decomposition of the empirical covariance matrix. Two P&L methods are compared:

Sensitivity-based: linear approximation using PC deltas
Full revaluation: swap repriced from scratch under each simulated curve

Historical Simulation VaR
Actual historical risk factor observations are replayed directly on the current portfolio. Again compared under sensitivity-based and full revaluation.

