# FreshOps

**Stockout-aware demand forecasting and perishable inventory planning for small grocery shops.**

FreshOps is being built to help an owner answer: **What should I order tomorrow, given my current stock, expiry dates, and budget?**

## Status

Discovery and product definition. There is no runnable application or trained model yet. Planned features and impact claims below are goals, not completed results.

## The problem

Over-ordering creates waste; under-ordering loses sales. Sales history also hides demand when an item sells out. FreshOps will connect demand estimates to reviewable order recommendations rather than stopping at a forecast chart.

## Planned workflow

Import sales and availability records → inspect data quality → forecast next-day demand → account for usable inventory and constraints → review and approve a draft order → record outcomes.

The initial scope is one small grocery shop, 20–50 perishable products, and one supplier per product. Owners remain in control of ordering.

## Planned engineering

- **Product:** React, TypeScript, and a responsive owner dashboard.
- **Backend:** FastAPI, PostgreSQL, validated imports, inventory transactions, and organization-scoped authorization.
- **ML:** forecasting baselines, LightGBM quantile models, chronological evaluation, and explicit stockout-recovery assumptions.
- **Decisions:** a baseline replenishment policy followed by constrained allocation with OR-Tools.
- **Reliability:** Celery/Redis jobs, safe retries, deduplication, model versions, and recovery checks.
- **Delivery:** Docker, automated checks, a deployed demo, and reproducible evaluation.

These choices are provisional until implementation validates them. Start with a modular backend and a separate worker; scale from measurements.

## Documentation

- [Problem, scope, and acceptance criteria](docs/problem-statement.md)
- [120-session project roadmap](docs/roadmap.md)
- [Day 1 record](docs/progress/day-001.md)
- [Contribution and commit workflow](CONTRIBUTING.md)

## Data and evidence

The intended benchmark is [FreshRetailNet-50K](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K), whose dataset card lists CC BY 4.0. Its quantities are normalized. Costs, expiry dates, supplier terms, and complete inventory records require labeled demo assumptions or consenting pilot data.

Benchmark results, simulated business outcomes, and real pilot outcomes will be reported separately. No reduction in waste, cost, or shortages has been demonstrated yet.

## Development

Day 1 contains documentation and repository conventions only. Application setup commands will be added when the backend and frontend exist. Raw datasets, secrets, and generated model artifacts are excluded from Git.
