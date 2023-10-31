# JOIN SERVICE

This `transformers` service can be used to join multiple dataframes.

Remember to call the right service:
```JAVA
String service = "transformers";
```

## For single input file:

Here is a sample json params:

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
    }]
}
```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `join` to join two tables
  - *right_table*: the arrow file that acts like the right table.
  - *on*: `name1=name2` meaing that ON Table1.name1 == Table2.name2
  - *on*: Fully support for multiple conditions. For example `"on": "(T1.A = T2.B) and (T1.C = T2.D or T1.E == T2.F)"`.
  - I believe that the join with OR will be much faster than real SQL.
  - *how*: There are 6 types: "inner", "left", "outer", "semi", "anti", "cross"

**Cách gọi nhiều tables:**
```JSON
    {
        "operator": "join",
        "options":
        {
            "right_table": "table2.arrow",
            "on": "name1=name2",
            "how": "inner"
        }
    },
    {
        "operator": "join",
        "options":
        {
            "right_table": "table3.arrow",
            "on": "name1=name3",
            "how": "left"
        }
    }
```

## For multiple input files:
```JSON
{
    "input": ["/path1/file1.arrow", "/path2/file2.arrow"],
    "output": "output.arrow",
    "operations": [
    {
        "operator": "join",
        "options":
        {
            "right_table": ["/path1/right1.arrow", "/path2/right2.arrow"],
            "on": "hash=hash",
            "how": "left"
        }
    }]
}
```

Notices that all files in the input list must have the same `SCHEMA` and all files in right_table must also have the same `SCHEMA`.
