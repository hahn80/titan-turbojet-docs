# Cross Table


A cross table (also known as a contingency table, cross-tabulation, or crosstab) is a statistical tool used to summarize the relationship between two or more categorical variables. It presents the value (count, sum, mean, ...) of different combinations of values from these variables, making it easier to analyze their interaction.

This `statistics` service can be used to multiple columns at onces.

Here is a sample json params: In this example we aggregate data for each pair State and City that match a unique pair of CustomerName and Category.
	

```JSON
{
  "input": "/tmp/tmpine4v1_j",
  "output": "/tmp/tmp249q_sd8",
  "operations": [
    {
      "operator": "cross_table",
      "options": {
        "index": [
          "State",
          "City"
        ],
        "columns": [
          "CustomerName",
          "Category"
        ],
        "aggregations": {
          "Sales": [
            {
              "func": "sum",
              "suffix": "",
              "args": []
            },
            {
              "func": "max",
              "suffix": "",
              "args": []
            }
          ],
          "Quantity": [
            {
              "func": "sum",
              "suffix": "",
              "args": []
            }
          ]
        }
      }
    }
  ]
}
```


***Chú thích:***

- *input*: the input arrow file.
- *output*: the output result.
- *operator*: `cross_table` the operator to run.
- *options*: a Hashmap that contains 3 keys: index, columns, and aggregations
  - *index*: List of String, forms the index to match.
 - *columns*: List of String, the column value to cross
 - *aggregations*: a Hashmap of column_name and list of information for aggregation function.


