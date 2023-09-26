# Outlier for transformers

This is an outlier API for `transformers` service.

Here is a sample json params:
```json
{
    "input": "outlier.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["x1", "x2", "x3"]
        }
    },
    {
        "operator": "outlier",
        "options":
        {
            "outlier_name": "iqr",
            "columns": ["x3"],
            "iqr_levels": [0.8, 1.2, 1.5]
        }
    }]
}
```
**Chú thích:**

- *outlier_name* has the following options:
  * iqr: Outlier using IQR
  * z_scores: Outlier using Z-Scores.
- *columns*: List of columns to check outliers.
- Kiểu dữ liệu cho columns là numerical (int, float).
- `iqr_levels`: List of quartiles multiples (float type).


**Output results:**

```sh
x1        ┆ x2        ┆ x3        ┆ x3_outlier_rank │
│ ---       ┆ ---       ┆ ---       ┆ ---             │
│ f64       ┆ f64       ┆ f64       ┆ cat             │
╞═══════════╪═══════════╪═══════════╪═════════════════╡
│ -0.948504 ┆ -0.407295 ┆ 1.850977  ┆ H1              │
│ -0.139155 ┆ -1.907192 ┆ 2.418141  ┆ H2              │
│ 0.102     ┆ 0.665053  ┆ -2.920141 ┆ L3              │
│ -1.419125 ┆ -0.34537  ┆ -2.336888 ┆ L2              │
```

*x3_outlier_rank*: Chỉ ra mức độc cho outlier: H1, H2, H3: mức outlier cao. L1, L2, L3: mức thấp. Nếu không cần cột này thì có thể bỏ qua.

