# ForexCube 📊

> Systematic Forex strategy selection validated by **real broker
> execution** — not by backtests.

## The thesis

Most algorithmically-generated trading strategies — even after extensive
robustness testing — fail when they meet real execution: live spreads and
commissions, slippage and latency, broker-specific pricing, and the subtle
gaps between historical data and live feeds. ForexCube inverts the usual
validation order. Candidate strategies must first **survive months of live
execution on demo accounts under real broker conditions**; only the
survivors, measured by proprietary scoring that explicitly discounts
immature evidence and demo-to-real degradation, become eligible for
capital.

Three working hypotheses drive the system, continuously re-tested against
the data the operation itself generates:

1. **Live execution beats simulation** as a predictor of real-account
   performance.
2. **Large populations of mutually uncorrelated strategies** compose into
   portfolios with lower drawdown, less stagnation, and better
   risk-adjusted return.
3. **Long and short sides are independent entities** — decomposing them
   doubles selection granularity and cuts asymmetric regime exposure.

## The pipeline

```text
Generate         evolutionary search + robustness filters
    │
    ▼
Validate LIVE    demo accounts under real broker conditions
    │
    ▼
Analyze          metrics · regimes · correlation  ◀────────────┐
    │                                                          │
    ▼                                                          │
Score            strategy + portfolio scores (reality haircut) │
    │                                                          │
    ▼                                                          │
Compose          low-correlation · redundancy-filtered         │
    │                                                          │
    ▼                                                          │
Operate          real capital · monitor · rotate ──────────────┘
                                         every fill feeds back
```

Two proprietary scoring layers are the core IP: a **Strategy Score** that
combines observed quality with the *maturity of the evidence* (a young
strategy with a great short sample deliberately cannot score high), and a
**Portfolio Score** that grades the assembled portfolio as a single
synthetic strategy, penalizing concentration, currency imbalance, regime
fragility, and dependence on a few leaders. Before any score is computed,
demo results are marked down by a **reality haircut** calibrated
per-symbol/per-direction against a live sentinel account with real money —
the system institutionalizes skepticism about its own data.

## The platform

A proprietary end-to-end stack — analytics backend, operator interface,
deployment infrastructure, and a managed fleet (strategy-generation
machines and trading VPSes) under infrastructure-as-code governance with
full observability.

| Repo | What it is |
|------|------------|
| `operations_api` | Analytics backend — ingestion/sync, strategies, portfolios, scoring, market regimes, correlation, redundancy filtering |
| `cube_ui` | The operator interface |
| `production_server` | Zero-downtime deploy orchestration (dev/stg/prod) |
| `fleet` | Infrastructure-as-code for the machine fleet + observability |
| `tasks` | Specs, planning, and work tracking |

## ⚙️ Engineering excellence

We build like an institution, not a script:

- **Spec-driven development** — every feature specified, planned, and
  reviewed before implementation
- **Full CI/CD automation** — gated, multi-stage promotion across three
  environments, with an end-to-end parity suite (functional + visual)
  as the release gate
- **Automated quality & security gates** — linting, type safety, static
  analysis, and secret scanning on every change
- **Comprehensive test coverage** and reproducible regression baselines
- **End-to-end observability** — metrics, logging, and alerting across
  services and the trading fleet
- **Documentation as code** — per-domain architecture docs and runbooks
  maintained in the same PRs as the behavior they describe

## 🚀 Direction

Scale the validated-strategy population, deepen the regime-aware scoring
layers, automate portfolio rotation, and — conditional on sustained
performance — open the structure to external capital.

## 📫 Contact

- **Project Lead**: João Ariedi
- **Email**: fxcube@pm.me

---

**Note**: ForexCube is a proprietary platform for systematic trading
strategy evaluation and automated trading operations. Statements here
describe design objectives and methodology, not realized performance or
any assurance of results. This organization hosts only public-facing
information; methodology detail, infrastructure, and performance data are
confidential.
