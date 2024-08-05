# JOIN SERVICE

This `transformers` service can be used to join multiple dataframes.

Remember to call the right service:
```JAVA
String service = "transformers";
```

## For single pivot file:

Here is a sample json params:

```JSON
{
    "input": "blocks.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "pivot",
        "options":
        {
            "index": "City",
            "columns": ["Category"],
            "values": ["Quantity"],
            "agg_func": "max"
        }
    }]
}
```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `pivot` to cross values of a table.
  - *index*: the index column (String Type).
  - *columns*: the list of columns List<String>.
  - *valueson*: the list of values List<String>.
  - *agg_func*: Support only one agg function of the form {‘min’, ‘max’, ‘first’, ‘last’, ‘sum’, ‘mean’, ‘median’, ‘len’}. 



## For multiple pivot file:

Here is a sample json params:

```JSON
{
    "input": "blocks.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "multiple_pivot",
        "options":
        {
            "index": "City",
            "columns": ["Category"],
            "values": [
                    {"field": "Quantity", "agg_func": "max"},
                    {"field": "Quantity", "agg_func": "sum"},
                ]
        }
    }]
}
```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `pivot` to cross values of a table.
  - *index*: the index column (String Type).
  - *columns*: the list of columns List<String>.
  - *valueson*: the list of values List<Map<String, String>>. We can have multiple agg functions for one value!


