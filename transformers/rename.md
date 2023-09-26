# RENAME SERVICE

This is a small service to rename the columns of a database. It can be used right after `join` or `groupby`.

Here is a sample json input:

```JSON
{
    "input": "blocks.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "join",
        "options":
        {
            "right_table": "trans.arrow",
            "on": "hash=hash",
            "how": "left"
        }
    },
    {
        "operator": "rename",
        "options":
        {
            "mapping":
            {
                "column_0": "name1",
                "input_value": "name2",
                "output_value": "name3"
            }
        }
    }]
}
```

**Chú thích:**

- `options`: *mapping* is a dictionary to rename from old keys to new values. If the key does not exist in a table, we will ignore this key.

We can also use `rename` right after groupby. Here is an example.

```JSON
{
    "input": "blocks.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "join",
        "options":
        {
            "right_table": "trans.arrow",
            "on": "hash=hash",
            "how": "left"
        }
    },
    {
        "operator": "groupby",
        "options":
        {
            "by": "timestamp_month",
            "aggregates":
            {
                "size": [{"func": "mean"},{"func": "quantile", "args": { "quantile": 0.75, "interpolation": "linear"}}],
                "weight": [{"func": "sum"}],
                "input_value": [{"func": "var"}, {"func": "std"}],
                "output_value": [{"func": "min"}, {"func": "max"}],
                "fee": [{"func": "count"}]
            }
        }
    },
    {
        "operator": "rename",
        "options":
        {
            "mapping":
            {
                "mean(size)": "name1",
                "std(input_value)": "name2",
                "min(output_value)": "name3"
            }
        }
    }]
}
```
