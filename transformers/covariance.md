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
┌───────────┬───────────┬──────────┬──────────┬──────────┬──────────┬──────────┐
│ x1        ┆ x2        ┆ const    ┆ coeffici ┆ feature_ ┆ degrees_ ┆ sigma_sq │
│ ---       ┆ ---       ┆ ---      ┆ ents     ┆ names    ┆ of_freed ┆ uared    │
│ f64       ┆ f64       ┆ f64      ┆ ---      ┆ ---      ┆ om       ┆ ---      │
│           ┆           ┆          ┆ f64      ┆ str      ┆ ---      ┆ f64      │
│           ┆           ┆          ┆          ┆          ┆ f64      ┆          │
╞═══════════╪═══════════╪══════════╪══════════╪══════════╪══════════╪══════════╡
│ 0.004684  ┆ -0.001881 ┆ -0.02413 ┆ 0.578219 ┆ x1       ┆ 24.97045 ┆ 0.441799 │
│           ┆           ┆ 6        ┆          ┆          ┆ 1        ┆          │
│ -0.001881 ┆ 0.019852  ┆ -0.11713 ┆ 0.963632 ┆ x2       ┆ 24.97045 ┆ 0.441799 │
│           ┆           ┆ 3        ┆          ┆          ┆ 1        ┆          │
│ -0.024136 ┆ -0.117133 ┆ 1.005012 ┆ -4.19008 ┆ const    ┆ 24.97045 ┆ 0.441799 │
│           ┆           ┆          ┆ 2        ┆          ┆ 1        ┆          │
└───────────┴───────────┴──────────┴──────────┴──────────┴──────────┴──────────┘
```

This convariance table will be used to determine the determine the most useful range of prediction when users alter their wanted inputs.


# How to use covariance service output?


The covariance service output will be used to determine the strategy for any linear model from which the clients want to check the possible outcome of the prediction.

Now we have to use **CONVERTERS** service to continue in the next step. To reduce the computional time, save the covariance service into arrow file and call it whenever you want.

This is a prediction analysis service for `converters`.

String service_name = "converters";

and the json looks like:

```JSON
{
  "input": "/tmp/tmpok6y_n8q",
  "output": "/tmp/tmpaxguifb5",
  "operations": [
    {
      "operator": "prediction_analysis",
      "options": {
        "confidence_level": 0.8,
        "input_values": {
          "x1": 5,
          "x2": 6
        }
      }
    }
  ]
}
```

# Giải thích các tham số:

- `input_values`: HashMap<String, Float>: the values of the inputs variables that the clients want to adjust.

The result will give you the predicted value; the lower and upper range with the confidence level.
