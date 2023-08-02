# Transformer service: DATE_TRUNC

This function plays similar functionality as date_trunc in SQL.

Here is a sample json:

```JSON
{
    "input": "left.arrow",
    "output": "output.arrow",
    "batch_size": 1000,
    "reformat_string": false,
    "operations": [
    {
        "operator": "date_trunc",
        "options":
        {
            "ts": "cr7.cre_d",
            "freq": "month",
            "alias": "my_month"
        }
    }]
}
```

**Chú thích:**

- `ts`: string type, name of the datetime column.
- `freq`: string type, frequency to truncate the datetime. We support "second", "minute", "hour", "day", "week", "month", "quarter", "year".
- `alias`: This is optinal paramerter, if available we will rename the output column as your alias. If alias is `null`, the output column must be `{ts}_{freq}` 

**Output:**
The original dataframe + extra column of truncated datetime.
