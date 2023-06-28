# THIS IS OUTLIER SERVICE

Note that, we need to run outlier as `converter` service to get the output as JSON format.

## Outlier
Here is a sample json params:
```json
{
    "input": "outlier.arrow",
    "output": "output.json",
    "operations": [
        {
            "operator": "select",
            "options": {"columns": ["x1", "x2", "x3"]}
        },
        {
            "operator": "outlier",
            "options": {"outlier_name": "iqr", "columns": ["x3"], "weak_threshold": 2, "strong_threshold": 4}
        }
    ]
}
```
**Chú thích:**

- *outlier_name* has the following options:
  * iso_forest: Isolation Forest Algorithm
  * local_factor: Unsupervised Outlier Detection using the Local Outlier Factor
  * one_svm: Unsupervised Outlier Detection using SVM
  * ledoit_wolf: Oulier using LedoitWolf estimator
  * kernel_density: Outlier using Kernel Density Estimation
  * iqr: Outlier using IQR
  * z_scores: Outlier using Z-Scores.
- *columns*: Currently suport only one column!
- Kiểu dữ liệu cho columns là numerical (int, float).


**Cách hiển thị kết quả:**

```JSON
{
    "metric": {
        "x3__mean": 0.1531066141958941,
        "x3__median": 0.10696155773352142,
        "x3__max": 2.6955843375750215,
        "x3__min": -2.6220521544674997,
        "x3__count": 1010,
        "x3__q1": -0.620713585420005,
        "x3__q3": 0.7304041137578925
    },
    "outlier": [
        -2.9201413977115127,
        10.595118745616753,
        9.960805793952636,
        9.817926946321318,
        2.9639336673826846,
        11.68323968932216,
        3.525793946007154,
        9.683332970377903,
        11.249551191255689,
        2.7667969473100356,
        10.483644128344128,
        -2.772068452531892,
        -2.9133493351693915,
        -2.712787421450937,
        11.630283343752359,
        2.8488049578844215,
        12.689992360538431,
        11.479419516535614
    ]
}
```
