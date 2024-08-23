# Pivot Service

This `transformers` service can be used to join multiple dataframes.

Remember to call the right service:
```JAVA
String service = "transformers";
```

## For single pivot file:

Here is a sample json params:

```JSON
{
  "input": "/tmp/tmpvvaipmar",
  "output": "/tmp/tmpm58a06ac",
  "operations": [
    {
      "operator": "pivot",
      "options": {
        "index": [
          "City"
        ],
        "columns": [
          "Category"
        ],
        "values": [
          "Quantity"
        ],
        "agg_func": "max"
      }
    }
  ]
}
```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `pivot` to cross values of a table.
  - *index*: the index column (List of String Type or String).
  - *columns*: the list of columns List<String>.
  - *valueson*: the list of values List<String>.
  - *agg_func*: Support only one agg function of the form {‘min’, ‘max’, ‘first’, ‘last’, ‘sum’, ‘mean’, ‘median’, ‘len’}. 

The output:
```
┌─────────────┬─────────────┬───────────┬──────┬──────────┬──────┐
│ City        ┆ Electronics ┆ Furniture ┆ Toys ┆ Clothing ┆ Food │
│ ---         ┆ ---         ┆ ---       ┆ ---  ┆ ---      ┆ ---  │
│ str         ┆ i64         ┆ i64       ┆ i64  ┆ i64      ┆ i64  │
╞═════════════╪═════════════╪═══════════╪══════╪══════════╪══════╡
│ Los Angeles ┆ 18          ┆ 12        ┆ 5    ┆ 12       ┆ 7    │
│ Chicago     ┆ 17          ┆ 14        ┆ 18   ┆ 18       ┆ 14   │
│ New York    ┆ 11          ┆ 12        ┆ 18   ┆ 19       ┆ 17   │
│ Houston     ┆ 16          ┆ 17        ┆ 18   ┆ 17       ┆ 6    │
│ Phoenix     ┆ 12          ┆ 18        ┆ 19   ┆ 17       ┆ 12   │
└─────────────┴─────────────┴───────────┴──────┴──────────┴──────┘

```


## For multiple pivot file:

Here is a sample json params:

```JSON
{
  "input": "/tmp/tmp6z9297m5",
  "output": "/tmp/tmplh9spu1z",
  "operations": [
    {
      "operator": "multiple_pivot",
      "options": {
        "index": [
          "City"
        ],
        "columns": [
          "Category"
        ],
        "values": [
          {
            "field": "Quantity",
            "agg_func": "max"
          },
          {
            "field": "Quantity",
            "agg_func": "min"
          },
          {
            "field": "Sales",
            "agg_func": "sum"
          }
        ]
      }
    }
  ]
}

```

***Chú thích:***

- *input*: the input arrow file plays the role as the left table.
- *output*: the output result after joining.
- *operation*: `pivot` to cross values of a table.
  - *index*: the index column (List of String Type or String).
  - *columns*: the list of columns List<String>.
  - *valueson*: the list of values List<Map<String, String>>. We can have multiple agg functions for one value!


The output:
```
Columns output: ['City', 'Electronics_max_Quantity', 'Furniture_max_Quantity', 'Toys_max_Quantity', 'Clothing_max_Quantity', 'Food_max_Quantity', 'Electronics_sum_Quantity', 'Furniture_sum_Quantity', 'Toys_sum_Quantity', 'Clothing_sum_Quantity', 'Food_sum_Quantity', 'Electronics_sum_Sales', 'Furniture_sum_Sales', 'Toys_sum_Sales', 'Clothing_sum_Sales', 'Food_sum_Sales']

```
