# FORECAST_BY: TRANSFORMERS SERVICE

Use the `transformers` API service to call the function.

Here is a json sample
```JSON
{
    "input": "tsdata.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["date", "price", "supply", "prod_id", "prod_name"]
        }
    },
    {
        "operator": "forecast_by",
        "options":
        {
            "columns": ["price", "supply"],
            "freezing": ["prod_id", "prod_name"],
            "ds": "date",
            "changepoint_prior_scale": 0.01,
            "daily_seasonality": true,
            "freq": "H",
            "periods": 168
        }
    }]
}
```

# Params Explanation:

For operation **forecast_by**, we need to understand the following things:
- `columns`: The list of columns to predict (can be multiple cols).
- `freezing`: The list of freezing columns (groupby and then make prediction for each group).
- `ds`: Name of your timestamp in your database.
-  `freq`: The frequency of your timeseries data `ds`. We only accept **H** (for hourly); **D** (for daily); **W** (for weekly); and **M** (for monthly).
-  `periods`: the length of your future time series wanted to be predicted. 168 hours mean 7 days or one week.
-  `changepoint_prior_scale`: The float value from 0.005 to 0.5. If this value is higher, it is likely to create more complex model to predict complex data.


# Output result
The output result will be an ARROW file that contains the aggregated predictions for each `freezing` group.
