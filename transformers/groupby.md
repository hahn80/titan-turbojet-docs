# GROUPBY SERVICE

This is a `transformers` service can be used right after `join` operation.

Here is a sample json params:

```JSON
{
  "input": "/tmp/tmpzkkflhlp",
  "output": "/tmp/tmp9nba1aia",
  "operations": [
    {
      "operator": "groupby",
      "options": {
        "by": "Product",
        "aggregates": {
          "Quantity": [
            {
              "func": "mean",
              "alias": "Quantity_mean"
            },
            {
              "func": "quantile",
              "args": {
                "quantile": 0.75,
                "interpolation": "linear"
              },
              "alias": "Quantity_quantile75"
            },
            {
              "func": "quantile",
              "args": {
                "quantile": 0.95,
                "interpolation": "linear"
              },
              "alias": "Quantity_quantile95"
            }
          ],
          "Weight": [
            {
              "func": "sum",
              "alias": "Weight_sum"
            }
          ],
          "Price": [
            {
              "func": "avg",
              "alias": "Price_avg"
            }
          ]
        }
      }
    }
  ]
}
```

**Chú thích:**

Tham số `by` có kiểu là string hoặc list of strings. Ví dụ: `"by": ["name1", "name2"]`.

Trong tham số aggregates, chúng ta cần chỉ ra các hàm aggregate cho từng column. Một columns có thể áp dụng nhiều hàm cùng một lúc.
Ví dụ:
```JSON
"size": [{"func": "mean"},{"func": "quantile", "args": { "quantile": 0.75, "interpolation": "linear"}}]
```
Tức là columns `size` sẽ được apply hai hàm liên tiếp là `mean` và `quantile`. Trong đó hàm quantile có các arguments để thay đổi nên phải bổ sung thêm vào.
Kết quả đầu ra sẽ hiển thị tên các cột `mean(size)` và `quantile(size)`.

**Supported functions:** count, mode, arg_max, arg_min, std, var, max, min, sum, mean, median, product, n_unique, null_count, firs, last, quantile, avg


**Tốc độ là tất cả:**

Để thấy được tốc độ tối đa của nó, cần kết hợp join với groupby. Dưới đây là một ví dụ.

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
    }]
}
```
