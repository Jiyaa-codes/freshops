# Problem statement: deciding tomorrow's perishable order

## Intended user and decision

The first intended user is the owner or stock manager of one small grocery shop with 20–50 perishable products. Each day they decide how much of each product to order before a supplier cutoff, using limited cash and storage.

**Problem hypothesis:** ordering from recent recorded sales alone can create avoidable shortages or waste because sales may be constrained by stockouts, while existing inventory may expire before it can be sold.

This hypothesis has not yet been validated with a shop owner. The project starts as a research/demo product until real user evidence exists.

## Why sales are not always demand

Illustrative fictional example: a shop sells its 20 available milk units by noon. The sales record says 20, but customers arriving later may still want milk. Treating 20 as the full day's demand risks repeating the shortage. Ordering more without checking tomorrow's usable stock, shelf life, and budget can create a different problem: waste.

FreshOps should represent this uncertainty and show the assumptions behind an order proposal. It must not present an estimate of hidden demand as an observed fact.

## Proposed daily workflow

1. The owner signs in and selects their shop.
2. They upload sales/availability records and review invalid rows or missing dates.
3. They record on-hand batches, expiry dates, supplier lead times, and costs.
4. A background job prepares next-day demand estimates and an order proposal.
5. They inspect quantities, forecast ranges, stock warnings, and budget usage.
6. They adjust quantities if needed and approve a saved draft order.
7. Later, they record sales and waste so the system can compare outcomes.

Approval records a decision inside FreshOps. It does not place an order with a supplier or spend money.

## First-release boundaries

| Included | Deferred |
|---|---|
| One-shop pilot/demo; isolated organization data | Multi-store stock transfers |
| 20–50 products and one supplier per product | Supplier bidding and dynamic pricing |
| Next-day planning with configurable lead time | Long-horizon supply-chain planning |
| CSV imports and manual inventory batches | Live POS integrations and offline sync |
| Baselines, uncertainty, and constrained recommendations | Deep-learning comparison and language assistant |
| Human review, overrides, and an audit trail | Automatic purchasing or supplier messages |

## Inputs and evidence boundaries

- **Benchmark:** the official [FreshRetailNet-50K dataset card](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/blob/main/README.md) supplies sales and stockout information. Its listed license is CC BY 4.0 and quantities are normalized.
- **Operational demo fixtures:** separately labeled costs, budgets, shelf lives, lead times, and inventory batches. These are assumptions, not fields claimed to come from the benchmark.
- **Pilot:** consented shop data, if a willing owner is found. Public benchmark performance does not establish local-shop performance.
- **Unknown demand:** natural stockouts conceal missed sales. Artificial masking can evaluate recovery on known withheld observations but does not establish the true demand of natural stockouts.

All features must be available at the decision cutoff. Future realized stockouts or tomorrow's observed weather cannot be used as if known when ordering.

## Acceptance criteria for the first complete workflow

- An owner can upload a documented CSV and identify rejected rows.
- Repeating an import cannot duplicate the same business records.
- Inventory accounts for batches and expiry under a documented consumption policy.
- A saved forecast includes its cutoff, model version, and uncertainty information.
- A proposal shows usable stock, proposed purchases, cost assumptions, and constraints.
- Approval is authorized, recorded once, and rejects or recalculates stale proposals.
- Failed or insufficient-data forecasts use a visible, documented fallback.
- A second organization cannot read or modify the first organization's records.
- A reproducible evaluation compares the proposal policy with a simple baseline.

## How success will be assessed

Forecasts: chronological validation, error and bias, quantile loss, and interval coverage. Decisions: simulated waste, shortage, cost, and fill rate under declared assumptions. Engineering: duplicate delivery, crash recovery, authorization, and concurrency checks. Product: observed user feedback if access is available.

There is no claimed improvement percentage today. Negative results and limitations will remain in the evaluation report.

## Open questions for Day 2

1. How does the owner decide order quantities now, and what is their cutoff?
2. Do they record when products sell out and what stock expires?
3. Which products cause the most costly mistakes?
4. Are pack sizes, minimum orders, lead times, or cold storage the binding constraints?
5. Would they try a CSV-based workflow, and what information would make a suggestion trustworthy?
