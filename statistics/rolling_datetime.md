# Rolling Datetime Functions

There are to advanced windows functions: `rolling_windows` and `rolling_datetime`

1. Rolling Datetime Function:

Hàm này tính các giá trị thống kê theo window trượt theo một cột nào đó.
Ví dụ: Nếu có data một cửa hàng theo ngày, cần tính tổng thu nhập của 5 ngày (hoặc một tuần, một tháng) liên tục trượt trên toàn bộ dữ liệu. Trong thống kê nó là một kiểu moving average.

This `statistics` service can be used to multiple columns at onces.

Here is a sample json params:
	

```JSON
{
  "input": "/tmp/tmpx03792y2",
  "output": "/tmp/tmplx932_y3",
  "operations": [
    {
      "operator": "rolling_datetime",
      "options": {
        "Sales": [
          {
            "func": "rolling_sum",
            "alias": "Sales_rolling_sum",
            "window_size": "1w",
            "closed": "right",
            "by": "OrderDate"
          }
        ],
        "Quantity": [
          {
            "func": "rolling_max",
            "alias": "Quantity_rolling_max",
            "window_size": "3d",
            "closed": "right",
            "by": "OrderDate"
          }
        ]
      }
    }
  ]
}
```


***Chú thích:***

- *input*: the input arrow file.
- *output*: the output result.
- *operator*: `rolling_datetime` the operator to run.
- *options*: is a dictionary of key=column_name, values is the list of operations for that column_name.
  - *func*: the function to compute the moving function (String), support:
	"rolling_max",
    "rolling_mean",
    "rolling_std",
    "rolling_min",
    "rolling_median",
    "rolling_sum",
    "rolling_var",
    "rolling_quantile",
 - *alias*: alias name for the result (String)
 - *window_size*: The size of rolling windows (Unit length for datetime).

* 1ns (1 nanosecond)
* 1us (1 microsecond)
* 1ms (1 millisecond)
* 1s (1 second)
* 1m (1 minute)
* 1h (1 hour)
* 1d (1 calendar day)
* 1w (1 calendar week)
* 1mo (1 calendar month)
* 1q (1 calendar quarter)
* 1y (1 calendar year)

 - *closed*: (String) support 3 values "right", "left", "both"
	left -> moving from (Time - Window) to (Time)
	right -> moving from (Time) to (Time + Window)
	both -> moving from (Time - Window) to (Time + Window)
 - *by*: (String) the name of the datetime column to roll the window.

Example:

Input:
┌─────────┬─────────────────────┬────────┬──────────┐
│ OrderID ┆ OrderDate           ┆ Sales  ┆ Quantity │
│ ---     ┆ ---                 ┆ ---    ┆ ---      │
│ i64     ┆ datetime[μs]        ┆ f64    ┆ i64      │
╞═════════╪═════════════════════╪════════╪══════════╡
│ 21      ┆ 2001-01-21 00:00:00 ┆ 849.47 ┆ 19       │
│ 13      ┆ 2001-01-13 00:00:00 ┆ 94.27  ┆ 7        │
│ 74      ┆ 2001-03-15 00:00:00 ┆ 275.17 ┆ 7        │
│ 89      ┆ 2001-03-30 00:00:00 ┆ 53.05  ┆ 7        │
│ 64      ┆ 2001-03-05 00:00:00 ┆ 710.72 ┆ 3        │
│ 91      ┆ 2001-04-01 00:00:00 ┆ 284.32 ┆ 6        │
│ 9       ┆ 2001-01-09 00:00:00 ┆ 686.31 ┆ 8        │
│ 8       ┆ 2001-01-08 00:00:00 ┆ 350.73 ┆ 4        │
│ 55      ┆ 2001-02-24 00:00:00 ┆ 721.85 ┆ 2        │
│ 54      ┆ 2001-02-23 00:00:00 ┆ 695.47 ┆ 19       │
└─────────┴─────────────────────┴────────┴──────────┘

Output:

┌─────────┬────────────────┬────────┬──────────┬───────────────┬───────────────┐
│ OrderID ┆ OrderDate      ┆ Sales  ┆ Quantity ┆ Sales_rolling ┆ Quantity_roll │
│ ---     ┆ ---            ┆ ---    ┆ ---      ┆ _sum          ┆ ing_max       │
│ i64     ┆ datetime[μs]   ┆ f64    ┆ i64      ┆ ---           ┆ ---           │
│         ┆                ┆        ┆          ┆ f64           ┆ i64           │
╞═════════╪════════════════╪════════╪══════════╪═══════════════╪═══════════════╡
│ 1       ┆ 2001-01-01     ┆ 315.21 ┆ 7        ┆ 315.21        ┆ 7             │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 2       ┆ 2001-01-02     ┆ 922.94 ┆ 4        ┆ 1238.15       ┆ 7             │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 3       ┆ 2001-01-03     ┆ 483.92 ┆ 19       ┆ 1722.07       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 4       ┆ 2001-01-04     ┆ 785.1  ┆ 8        ┆ 2507.17       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 5       ┆ 2001-01-05     ┆ 458.02 ┆ 4        ┆ 2965.19       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ …       ┆ …              ┆ …      ┆ …        ┆ …             ┆ …             │
│ 96      ┆ 2001-04-06     ┆ 324.03 ┆ 11       ┆ 2081.8        ┆ 16            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 97      ┆ 2001-04-07     ┆ 159.49 ┆ 14       ┆ 1954.45       ┆ 16            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 98      ┆ 2001-04-08     ┆ 597.77 ┆ 3        ┆ 2186.39       ┆ 14            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 99      ┆ 2001-04-09     ┆ 679.59 ┆ 16       ┆ 2814.93       ┆ 16            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 100     ┆ 2001-04-10     ┆ 29.05  ┆ 14       ┆ 2618.97       ┆ 16            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
└─────────┴────────────────┴────────┴──────────┴───────────────┴───────────────┘


