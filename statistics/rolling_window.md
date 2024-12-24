# Rolling Windows Functions

There are to advanced windows functions: `rolling_windows` and `rolling_datetime`

1. Rolling Window Function:

Hàm này tính các giá trị thống kê theo window trượt theo một cột nào đó.
Ví dụ: Nếu có data một cửa hàng, cần tính tổng thu nhập của 5 records liên tục trượt trên toàn bộ dữ liệu. Trong thống kê nó là một kiểu moving average.

This `statistics` service can be used to multiple columns at onces.

Here is a sample json params:
	

```JSON
{
  "input": "/tmp/tmpoqqv5b0a",
  "output": "/tmp/tmpxu2ce3wk",
  "operations": [
    {
      "operator": "rolling_window",
      "options": {
        "Sales": [
          {
            "func": "rolling_sum",
            "alias": "Sales_rolling_sum",
            "window_size": 10
          }
        ],
        "Quantity": [
          {
            "func": "rolling_max",
            "alias": "Quantity_rolling_max",
            "window_size": 10
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
- *operator*: `rolling_window` the operator to run.
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
 - *window_size*: The size of rolling windows (int)

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
│ 1       ┆ 2001-01-01     ┆ 927.08 ┆ 8        ┆ null          ┆ null          │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 2       ┆ 2001-01-02     ┆ 683.8  ┆ 7        ┆ null          ┆ null          │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 3       ┆ 2001-01-03     ┆ 528.43 ┆ 11       ┆ null          ┆ null          │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 4       ┆ 2001-01-04     ┆ 487.07 ┆ 18       ┆ null          ┆ null          │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 5       ┆ 2001-01-05     ┆ 381.23 ┆ 19       ┆ null          ┆ null          │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ …       ┆ …              ┆ …      ┆ …        ┆ …             ┆ …             │
│ 96      ┆ 2001-04-06     ┆ 31.97  ┆ 4        ┆ 3230.96       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 97      ┆ 2001-04-07     ┆ 396.14 ┆ 6        ┆ 2797.16       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 98      ┆ 2001-04-08     ┆ 142.1  ┆ 19       ┆ 2237.48       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 99      ┆ 2001-04-09     ┆ 725.58 ┆ 15       ┆ 2910.01       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
│ 100     ┆ 2001-04-10     ┆ 836.15 ┆ 8        ┆ 2957.26       ┆ 19            │
│         ┆ 00:00:00       ┆        ┆          ┆               ┆               │
└─────────┴────────────────┴────────┴──────────┴───────────────┴───────────────┘

