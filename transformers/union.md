# Union Service

This `transformers` service can be used to union multiple dataframes.

Here is a sample json params:

```JSON
{
    "input": "left.arrow",
    "output": "output.arrow",
    "batch_size": 32000,
    "reformat_string": true,
    "operations": [
    {
        "operator": "union",
        "options":
        {
            "right_table": "right.arrow",
            "left_columns": ["ma", "ten", "so_luong", "gia"],
            "right_columns": ["ma", "ten", "so_luong", "gia"],
            "keep_duplicate": true
        }
    }]
}
```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `union` to join two tables
  - *right_table*: the arrow file that acts like the right table.
  - *left_columns*: list of left columns.
  - *right_columns*: list of right columns (they should match the order of the left ones).
  - *keep_duplicate*: boolean :: if you want to keep duplicates set it true; default value = false

