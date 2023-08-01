# Transformer service to truncate dates

Here is a sample json:

```JSON
{
    "input": "left.arrow",
    "output": "output.arrow",
    "batch_size": 1000,
    "reformat_string": false,
    "operations": [
    {
        "operator": "date_truncation",
        "options":
        {
            "ts": "cr7.cre_d"
        }
    }]
}
```

**Chú thích:**

- "ts": string type, name of the datetime column.

**Output:**

The output name should be {ts}+'_month_stat' or {ts}_month_end
