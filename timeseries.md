# Time series prediction

Time series can be performed by running the `converter` service from our API backend.
Here is the json sample

```json
{
    "input": "tsdata.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["date", "price", "supply"]
        }
    },
    {
        "operator": "forecast",
        "options":
        {
            "columns": ["price", "supply"],
            "ds": "date",
            "changepoint_prior_scale": 0.01,
            "daily_seasonality": true,
            "freq": "H",
            "periods": 168
        }
    },
    {
        "operator": "ts_report",
        "options":
        {
            "columns": ["price", "supply"],
            "ds": "date",
            "freq": "H",
            "k": 3
        }
    }]
}
```

## Params Explanation:

For operation **forecast**, we need to understand the following things:
- `columns`: The list of columns to predict (can be multiple cols).
- `ds`: Name of your timestamp in your database.
-  `freq`: The frequency of your timeseries data `ds`. We only accept **H** (for hourly); **D** (for daily); **W** (for weekly); and **M** (for monthly).
-  `periods`: the length of your future time series wanted to be predicted. 168 hours mean 7 days or one week.

For the operation **ts_report**, we create a json report of prediction.
`columns`, `ds`, `freq' are similar to the above, but `k` is the integer which tells you how many lowest or highest data points that you want to extract.
For example, if k=3, then we will get 3 data points for the lowest and 3 data points for the highest.
