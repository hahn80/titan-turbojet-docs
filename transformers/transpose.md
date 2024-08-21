# Transpose Service

This `transpose` service can be used with the following json sample:
```JAVA
String service = "transformers";
```

Sample json params:

```json
{
  "input": "/tmp/tmpe_buah31",
  "output": "/tmp/tmpltjmzorz",
  "operations": [
    {
      "operator": "transpose",
      "options": {
        "include_header": true,
        "header_name": "columns",
        "header_columns": [
          "col_0",
          "col_1",
          "col_2",
          "col_3",
          "col_4",
          "col_5",
          "col_6",
          "col_7",
          "col_8",
          "col_9"
        ]
      }
    }
  ]
}
```
