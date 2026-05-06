# Results

This page summarizes the report-level findings produced by Step 06.

## Experimental contract

| Item | Value |
|---|---:|
| Tickers | AMZN, AAPL, GOOG, INTC, MSFT |
| LOB levels | 10 |
| Time bins | 46,800 |
| Bin width | 500ms |
| Feature tensor | `(46800, 5, 51)` |
| Horizons | `tau = 1, 2, 10, 20` |

## Main finding

Graph-robust training is the most consistent robust optimization component in this experiment. It improves degradation under graph perturbation for every tested horizon and lowers graph sensitivity across all tested horizons.

Feature-robust training is more mixed. It is not helpful for the shortest horizons, but it gives positive degradation improvement for longer horizons (`tau = 10, 20`) at the cost of larger clean-test error.

## Step 06 final ablation table

| tau | feature MSE degradation improvement | feature robust choice | graph MSE degradation improvement | graph sensitivity improvement | graph robust choice |
|---:|---:|---|---:|---:|---|
| 1 | -2.39226e-10 | rho=0.05,R=3 | 2.09721e-12 | 1.53701e-11 | rho=0.1,R=1 |
| 2 | -1.86677e-09 | rho=0.05,R=1 | 6.04494e-12 | 6.86322e-11 | rho=0.2,R=1 |
| 10 | 6.94136e-08 | rho=0.2,R=3 | 1.14795e-10 | 9.38278e-10 | rho=0.2,R=3 |
| 20 | 7.28205e-08 | rho=0.2,R=1 | 2.45670e-10 | 2.52479e-09 | rho=0.2,R=3 |

## Interpretation

The project should be presented as a stability study rather than a pure accuracy improvement study. Robust optimization does not uniformly improve clean prediction error. Its value is clearer under structured stress tests:

- feature perturbation stress tests;
- graph perturbation stress tests;
- sensitivity metrics;
- mask-aware and graph-aware ablations.

The cleanest final claim is:

> In this five-asset one-day LOBSTER sample, robustness to cross-asset graph misspecification is more stable than feature-level adversarial robustness.

## Limitations

- The public LOBSTER samples cover one trading day.
- Rolling graph experiments are not yet included in the main result.
- PnL is a diagnostic proxy, not a production backtest.
- Feature-robust PGDA requires calibrated adversarial evaluation; raw-gradient attacks can produce false zero-degradation results.
