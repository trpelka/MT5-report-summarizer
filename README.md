# MT5 Report Summarizer

Turn a raw MetaTrader 5 trade export into a one-page performance summary and an equity curve — in Colab, with nothing but pandas and matplotlib.

## What it calculates

| Metric | Definition |
| --- | --- |
| Total trades | Number of closed trades |
| Win rate | Winning trades / total trades |
| Gross profit | Sum of positive trades |
| Gross loss | Absolute sum of negative trades |
| Profit factor | Gross profit / gross loss |
| Net profit | Sum of all trades |
| Max drawdown | Largest peak-to-trough drop of the cumulative profit curve |

It then plots the cumulative profit (equity) curve over time.

## Quick start

1. Open [Google Colab](https://colab.research.google.com) and create a new notebook.
2. Paste the contents of [`report summarizer`](./report%20summarizer) into a cell.
3. Run it and upload your MT5 trade export as CSV.

The script uses `google.colab.files.upload()`, so it is intended for Colab. To run locally, replace the upload block with:

```python
import pandas as pd
df = pd.read_csv("your_export.csv")
```

## Input format

Any CSV with at least two columns: one profit/loss amount and one close time.

```csv
ticket,symbol,profit,close_time
101,EURUSD,120.50,2025-01-02 14:30:00
102,XAUUSD,-45.10,2025-01-03 09:15:00
```

Set the column names to match your export:

```python
PROFIT_COL = "profit"
TIME_COL = "close_time"
```

MT5 can export slightly different headers depending on version, language and the report type. Common alternatives are `Profit`, `Close Time`, `Zysk`, `Czas zamknięcia` — just point the two constants at the right columns.

## Output

```text
========================================
TRADING RESULTS SUMMARY
========================================
Total trades:     128
Win rate:         54.7%
Gross profit:     8420.00
Gross loss:       5110.00
Profit factor:    1.65
Net profit:       3310.00
Max drawdown:     -820.00
========================================
```

Plus an equity-curve chart.

## Notes

- Assumes one row per closed trade.
- Does not model commission, swap or partial closes; add those columns to `profit` first if you need net-of-cost numbers.
- A single uploaded file is processed per run.
- Max drawdown is calculated on cumulative closed-trade profit, not on account equity including open positions.
