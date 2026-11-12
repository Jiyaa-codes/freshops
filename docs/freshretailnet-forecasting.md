# FreshRetailNet-50K for next-day forecasting

Start with past sales and calendar features. Use stockout flags to identify historical periods when sales may understate demand, then compare a model that accounts for stockouts with a simple sales forecast. Tomorrow's recorded sales, stockouts, and weather are outcomes that arrive after the order decision.

## What the release contains

Dingdong's v1.0 release has about 50,000 store–product series. The expected row key is `(store_id, product_id, dt)`. Each row includes a daily sales total and hourly sales and stock-status arrays. The published Parquet splits are `train` (4,500,000 rows) and `eval` (350,000 rows). The dataset card lists [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Credit Dingdong and Wang et al., link the source and license, and identify modifications when sharing adapted data. [Dataset card](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/blob/main/README.md) · [Released files](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/tree/main/data)

The published sales amounts have been scaled. Keep them in **normalized sales units** throughout benchmark reporting. A value of `0.1` in the viewer cannot be translated into cartons, a supplier pack, or revenue from the released fields. [Dataset card](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/blob/main/README.md) · [Training data viewer](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/viewer/default/train)

## Field decisions at order time

For target date `D`, the first benchmark assumes a cutoff after the close of `D-1`. If a shop orders before closing, use the last complete day or separately timestamped observations available by its cutoff. Use the same availability rule in training and prediction.

| Released fields | FreshOps use |
|---|---|
| `dt` | Parse as a date. Derive target weekday and weekend from the calendar. Use actual dates for lags so a missing date does not become a one-day lag. |
| `store_id`, `product_id` | Group observations into series. Treat IDs as categorical if used in a pooled model. |
| `city_id`, `management_group_id`, `first_category_id`, `second_category_id`, `third_category_id` | Optional categorical context for a pooled model, provided the mapping is known at order time. |
| `sale_amount` | Observed daily sales label for date `D`. Use earlier dates for lagged sales and rolling averages. Sales during stockouts can understate demand. |
| `hours_sale` | Use completed hours to study intraday patterns or estimate sales missed during historical stockouts. Hours from the target day are unavailable at prediction time. |
| `stock_hour6_22_cnt`, `hours_stock_status` | Use lagged values as historical availability signals and use target-day annotations **afterward** for evaluation. The count covers a stated daytime window. Use hourly flags when checking whether sales were censored outside that window. |
| `holiday_flag` | Start with weekday and weekend derived from `dt`. Add the released flag only if its labeling can be reproduced from calendar information available before ordering. |
| `discount`, `activity_flag` | Historical context. Add target-day values only when a promotion schedule is known and recorded by the cutoff. |
| `precpt`, `avg_temperature`, `avg_humidity`, `avg_wind_level` | Leave out of the first baseline. Use lagged observations if useful, or archived forecasts issued by the cutoff for a future-weather experiment. The recorded target-day weather would leak future information. |

The training viewer provides a useful spot check. Store `0`, product `38`, on `2024-03-30` has a daytime stockout count of `0` while the hourly array begins with six `1` values. This illustrates why a zero daytime count alone is insufficient to label a full day as in stock. Confirm the hourly encoding and counting window across the sample on Day 4. [Training data viewer](https://huggingface.co/datasets/Dingdong-Inc/FreshRetailNet-50K/viewer/default/train)

## First experiments

1. **Establish an observed-sales baseline.** Compare the most recent completed day's sales, sales on the same weekday one week earlier, and a trailing seven-day average for each store–product pair. Track missing lags explicitly. Build validation windows in date order within `train`, and reserve `eval` for final reporting after model choices are fixed.
2. **Study stockout effects.** Report error and bias separately on days with no stockouts in the relevant hours and days with recorded stockouts. On a stockout day, `sale_amount` can score a prediction of observed sales, but it cannot tell us the true demand the shop missed.
3. **Evaluate demand recovery separately.** Hide selected hours from periods observed in stock and compare recovery estimates with those withheld sales. Fit recovery and preprocessing inside each training fold. During a backtest, construct every forecast's historical inputs only from information available at that forecast's cutoff. Artificially masked hours help measure recovery under that experiment's assumptions. Natural stockouts can occur precisely when demand is high. [Dataset paper](https://arxiv.org/html/2505.16319v1#S4)

## What an order recommendation still needs

To suggest physical purchase quantities, FreshOps needs shop-specific sales units, on-hand quantities by batch, expiry dates, scheduled receipts and supplier lead times, pack sizes, and purchasing costs and limits. The benchmark supports forecast development. These operating inputs must come from labeled demo assumptions or an actual shop. The source data covers a short period of Chinese online fresh retail, so evaluate transfer to the intended grocery workflow separately. [Dataset paper](https://arxiv.org/html/2505.16319v1#S3)

## Day 4 sample checks

Record the source revision and selection rule for a small set of complete store–product histories from `train`. Check date coverage and key uniqueness, missing versus zero sales, array lengths, stock-status values, and the daytime count against the hourly flags. Check whether hourly sales reconcile with `sale_amount` within a tolerance before choosing how to aggregate them. Leave `eval` outside the sample used to choose features or baselines.
