# THIS IS OUTLIER SERVICE WITH LEVELS

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
        "options":
        {
            "columns": ["x1", "x2", "x3"]
        }
    },
    {
        "operator": "outlier_level",
        "options":
        {
            "outlier_name": "iqr",
            "columns": ["x3"],
            "key": "x1",
            "weak_threshold": 2,
            "strong_threshold": 4,
            "metrics": ["q1", "q3", "count", "mean"]
        }
    }]
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
- *key*: Kiểu dữ liệu string.


**Cách hiển thị kết quả:**

```JSON
[
    {
        "key": "x1",
        "variable": "x3",
        "metrics": {
            "x3__q1": 0.1531066141958941,
            "x3__q3": 0.10696155773352142,
            "x3__count": 2.6955843375750215,
            "x3__mean": -2.6220521544674997
        },
        "outliers": {
            "weak": [
                {
                    "x1": 0.10199973800709858,
                    "x3": -2.9201413977115127,
                    "distance_to_mean": -3.073248011907407
                },
                {
                    "x1": -1.214420064287217,
                    "x3": 2.9639336673826846,
                    "distance_to_mean": 2.8108270531867907
                },
                {
                    "x1": -0.737853399785759,
                    "x3": 3.525793946007154,
                    "distance_to_mean": 3.3726873318112602
                },
                {
                    "x1": 1.2500971961669172,
                    "x3": 2.7667969473100356,
                    "distance_to_mean": 2.6136903331141417
                },
                {
                    "x1": -0.7288929329445405,
                    "x3": -2.772068452531892,
                    "distance_to_mean": -2.9251750667277863
                },
                {
                    "x1": -1.3607723233897147,
                    "x3": -2.9133493351693915,
                    "distance_to_mean": -3.0664559493652854
                },
                {
                    "x1": 0.1437557531264604,
                    "x3": -2.712787421450937,
                    "distance_to_mean": -2.865894035646831
                },
                {
                    "x1": -1.246953627803795,
                    "x3": 2.8488049578844215,
                    "distance_to_mean": 2.6956983436885276
                }
            ],
            "strong": [
                {
                    "x1": 6.292450957054243,
                    "x3": 10.595118745616753,
                    "distance_to_mean": 10.44201213142086
                },
                {
                    "x1": 6.849347488709529,
                    "x3": 9.960805793952636,
                    "distance_to_mean": 9.807699179756742
                },
                {
                    "x1": 6.448401550033933,
                    "x3": 9.817926946321318,
                    "distance_to_mean": 9.664820332125425
                },
                {
                    "x1": 4.327623761916106,
                    "x3": 11.68323968932216,
                    "distance_to_mean": 11.530133075126267
                },
                {
                    "x1": 8.222576652220543,
                    "x3": 9.683332970377903,
                    "distance_to_mean": 9.530226356182009
                },
                {
                    "x1": 6.484203823783212,
                    "x3": 11.249551191255689,
                    "distance_to_mean": 11.096444577059795
                },
                {
                    "x1": 6.995076337059516,
                    "x3": 10.483644128344128,
                    "distance_to_mean": 10.330537514148235
                },
                {
                    "x1": 7.605805614168475,
                    "x3": 11.630283343752359,
                    "distance_to_mean": 11.477176729556465
                },
                {
                    "x1": 6.245262031299696,
                    "x3": 12.689992360538431,
                    "distance_to_mean": 12.536885746342538
                },
                {
                    "x1": 8.602085586734685,
                    "x3": 11.479419516535614,
                    "distance_to_mean": 11.32631290233972
                }
            ]
        }
    }
]
```
