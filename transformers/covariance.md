# Covariance Service

This is a multiple regression service for `transformers`.

Here is a json sample
```JSON
{
  "input": "/tmp/tmphbq2y1ez",
  "output": "/tmp/tmpha_r5nqu",
  "operations": [
    {
      "operator": "covariance_analysis",
      "options": {
        "columns": [
          "x1",
          "x2"
        ],
        "target": "y",
        "alpha": 0.05,
        "mode": "covariance_inverse",
        "add_intercept": true
      }
    }
  ]
}
```


# Giải thích các tham số:

- `columns`: The list of columns List[String], datatype: Float, Integer.
- `target`: The name of the output value (String).
- `alpha`: Regularity constant for regression 0 <= alpha < infinity (Float).
- `mode`: For now, just use "covariance_inverse"
- `add_intercept`: Boolean (If we want to add bias to the regression)


# Output result
The output result will be an ARROW file that contains the predicted values.

```sh
┌───────────┬───────────┬──────────┬──────────────┬───────────────┬──────────┐
│ x1        ┆ x2        ┆ const    ┆ coefficients ┆ feature_names ┆ df       │
│ ---       ┆ ---       ┆ ---      ┆ ---          ┆ ---           ┆ ---      │
│ f64       ┆ f64       ┆ f64      ┆ f64          ┆ str           ┆ f64      │
╞═══════════╪═══════════╪══════════╪══════════════╪═══════════════╪══════════╡
│ 0.121644  ┆ -0.032253 ┆ 0.050284 ┆ 0.973021     ┆ x1            ┆ 9.633441 │
│ -0.032253 ┆ 0.121998  ┆ 0.003938 ┆ 0.982966     ┆ x2            ┆ 9.633441 │
│ 0.050284  ┆ 0.003938  ┆ 0.122917 ┆ -0.001899    ┆ const         ┆ 9.633441 │
└───────────┴───────────┴──────────┴──────────────┴───────────────┴──────────┘
```

This convariance table will be used to determine the determine the most useful range of prediction when users alter their wanted inputs.
