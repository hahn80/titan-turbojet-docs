# Regression Service

This is a multiple regression service for `transformers`.

Here is a json sample
```JSON
{
  "input": "/tmp/tmp3e28w5xo",
  "output": "/tmp/tmp8lemoa71",
  "operations": [
    {
      "operator": "linear_regression_analysis",
      "options": {
        "columns": [
          "x1",
          "x2"
        ],
        "target": "y",
        "alpha": 0.05,
        "confidence_level": 0.75,
        "mode": "statistics",
        "add_intercept": true,
        "method": "ols",
        "group_by": null
      }
    }
  ]
}
```

Sample json with group_by:

```JSON
{
  "input": "/tmp/tmpqupibdte",
  "output": "/tmp/tmplst4o5cl",
  "operations": [
    {
      "operator": "linear_regression_analysis",
      "options": {
        "columns": [
          "x1",
          "x2"
        ],
        "target": "y",
        "alpha": 0.05,
        "confidence_level": 0.75,
        "mode": "statistics",
        "add_intercept": true,
        "method": "ols",
        "group_by": "group"
      }
    }
  ]
}
```

# Giải thích các tham số:

- `columns`: The list of columns List[String], datatype: Float, Integer.
- `target`: The name of the output value (String).
- `alpha`: Regularity constant for regression 0 <= alpha < infinity (Float).
- `confidence_level`: Float number from 0.7 to 0.99
- `mode`: For now, just use "statistics"
- `add_intercept`: Boolean (If we want to add bias to the regression)
- `method`: String (choose one of "ols", "lasso", "ridge", "elastic_net")
- `group_by`: String (Name of group_by column; allows us to do regression on each group).


# Output result
The output result will be an ARROW file that contains the predicted values.

```sh
┌─────────┬──────────┬──────────┬────────────┬───────────┬───────────┬───────────┬───────────┬───────────┬───────────┐
│ r2      ┆ mae      ┆ mse      ┆ feature_na ┆ coefficie ┆ standard_ ┆ t_values  ┆ p_values  ┆ lower_bou ┆ upper_bou │
│ ---     ┆ ---      ┆ ---      ┆ mes        ┆ nts       ┆ errors    ┆ ---       ┆ ---       ┆ nds       ┆ nds       │
│ f64     ┆ f64      ┆ f64      ┆ ---        ┆ ---       ┆ ---       ┆ f64       ┆ f64       ┆ ---       ┆ ---       │
│         ┆          ┆          ┆ str        ┆ f64       ┆ f64       ┆           ┆           ┆ f64       ┆ f64       │
╞═════════╪══════════╪══════════╪════════════╪═══════════╪═══════════╪═══════════╪═══════════╪═══════════╪═══════════╡
│ 0.99629 ┆ 0.059321 ┆ 0.007983 ┆ x1         ┆ 0.973021  ┆ 0.031749  ┆ 30.647082 ┆ 6.2928e-1 ┆ 0.934156  ┆ 1.011886  │
│         ┆          ┆          ┆            ┆           ┆           ┆           ┆ 1         ┆           ┆           │
│ 0.99629 ┆ 0.059321 ┆ 0.007983 ┆ x2         ┆ 0.982966  ┆ 0.031795  ┆ 30.915418 ┆ 5.7904e-1 ┆ 0.944044  ┆ 1.021887  │
│         ┆          ┆          ┆            ┆           ┆           ┆           ┆ 1         ┆           ┆           │
│ 0.99629 ┆ 0.059321 ┆ 0.007983 ┆ const      ┆ -0.001899 ┆ 0.031915  ┆ -0.059504 ┆ 0.953767  ┆ -0.040967 ┆ 0.037169  │
└─────────┴──────────┴──────────┴────────────┴───────────┴───────────┴───────────┴───────────┴───────────┴───────────┘
```

- `r2`: R2 Scores (The same value for all rows)
- `mae`: Mean of absolute errors (The same value for all rows)
- `mse`: Mean of square errors (The same value for all rows)
- `feature_names`: List of input columns in regression. If `add_intercept=true`, it will add `const` to feature_names. This is a bias constant.
- `coefficients`: The regression coefficients. Use this for prediction.



In case you pass the group_by, the output has a group_by column to extract the information:

```sh
┌───────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬─────────┐
│ group ┆ r2       ┆ mae      ┆ mse      ┆ feature_ ┆ coeffici ┆ standard ┆ t_values ┆ p_values ┆ lower_bo ┆ upper_b │
│ ---   ┆ ---      ┆ ---      ┆ ---      ┆ names    ┆ ents     ┆ _errors  ┆ ---      ┆ ---      ┆ unds     ┆ ounds   │
│ i64   ┆ f64      ┆ f64      ┆ f64      ┆ ---      ┆ ---      ┆ ---      ┆ f64      ┆ f64      ┆ ---      ┆ ---     │
│       ┆          ┆          ┆          ┆ str      ┆ f64      ┆ f64      ┆          ┆          ┆ f64      ┆ f64     │
╞═══════╪══════════╪══════════╪══════════╪══════════╪══════════╪══════════╪══════════╪══════════╪══════════╪═════════╡
│ 1     ┆ 0.990398 ┆ 0.104993 ┆ 0.014695 ┆ x1       ┆ 0.995157 ┆ 0.081023 ┆ 12.28236 ┆ 0.006564 ┆ 0.865231 ┆ 1.12508 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆ 1        ┆          ┆          ┆ 3       │
│ 1     ┆ 0.990398 ┆ 0.104993 ┆ 0.014695 ┆ x2       ┆ 0.977494 ┆ 0.174269 ┆ 5.609113 ┆ 0.030345 ┆ 0.698042 ┆ 1.25694 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆ 6       │
│ 1     ┆ 0.990398 ┆ 0.104993 ┆ 0.014695 ┆ const    ┆ 0.014344 ┆ 0.094274 ┆ 0.152154 ┆ 0.893028 ┆ -0.13683 ┆ 0.16551 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆ 9       │
│ 1     ┆ 0.990398 ┆ 0.104993 ┆ 0.014695 ┆ x1       ┆ 0.995157 ┆ 0.081023 ┆ 12.28236 ┆ 0.006564 ┆ 0.865231 ┆ 1.12508 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆ 1        ┆          ┆          ┆ 3       │
│ 1     ┆ 0.990398 ┆ 0.104993 ┆ 0.014695 ┆ x2       ┆ 0.977494 ┆ 0.174269 ┆ 5.609113 ┆ 0.030345 ┆ 0.698042 ┆ 1.25694 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆          ┆ 6       │
│ …     ┆ …        ┆ …        ┆ …        ┆ …        ┆ …        ┆ …        ┆ …        ┆ …        ┆ …        ┆ …       │
│ 2     ┆ 0.999984 ┆ 0.005106 ┆ 0.000043 ┆ x2       ┆ 0.997441 ┆ 0.004279 ┆ 233.1257 ┆ 0.000018 ┆ 0.99058  ┆ 1.00430 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆ 65       ┆          ┆          ┆ 2       │
│ 2     ┆ 0.999984 ┆ 0.005106 ┆ 0.000043 ┆ const    ┆ -0.01759 ┆ 0.005344 ┆ -3.29315 ┆ 0.081144 ┆ -0.02616 ┆ -0.0090 │
│       ┆          ┆          ┆          ┆          ┆ 9        ┆          ┆ 4        ┆          ┆ 9        ┆ 3       │
│ 2     ┆ 0.999984 ┆ 0.005106 ┆ 0.000043 ┆ x1       ┆ 0.939217 ┆ 0.006559 ┆ 143.1874 ┆ 0.000049 ┆ 0.928698 ┆ 0.94973 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆ 9        ┆          ┆          ┆ 5       │
│ 2     ┆ 0.999984 ┆ 0.005106 ┆ 0.000043 ┆ x2       ┆ 0.997441 ┆ 0.004279 ┆ 233.1257 ┆ 0.000018 ┆ 0.99058  ┆ 1.00430 │
│       ┆          ┆          ┆          ┆          ┆          ┆          ┆ 65       ┆          ┆          ┆ 2       │
│ 2     ┆ 0.999984 ┆ 0.005106 ┆ 0.000043 ┆ const    ┆ -0.01759 ┆ 0.005344 ┆ -3.29315 ┆ 0.081144 ┆ -0.02616 ┆ -0.0090 │
│       ┆          ┆          ┆          ┆          ┆ 9        ┆          ┆ 4        ┆          ┆ 9        ┆ 3       │
└───────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴─────────┘
```
