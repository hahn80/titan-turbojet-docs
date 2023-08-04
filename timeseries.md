# Time series prediction

Time series can be performed by running the `converter` service from our API backend.
Here is the json sample

```json
{
    "input": "tsdata.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["date", "price", "supply", "prod_id", "prod_name"]
        }
    },
    {
        "operator": "forecast",
        "options":
        {
            "columns": ["price", "supply"],
            "freezing": ["prod_id", "prod_name"],
            "ds": "date",
            "changepoint_prior_scale": 0.01,
            "daily_seasonality": true,
            "freq": "H",
            "periods": 168
        }
    },
    {
        "operator": "ts_report",
        "options":
        {
            "columns": ["price", "supply"],
            "freezing": ["prod_id", "prod_name"],
            "ds": "date",
            "freq": "H",
            "k": 3
        }
    }]
}
```

## Params Explanation:

For operation **forecast**, we need to understand the following things:
- `columns`: The list of columns to predict (can be multiple cols).
- `freezing`: The list of freezing columns (copy over and not predicting).
- `ds`: Name of your timestamp in your database.
-  `freq`: The frequency of your timeseries data `ds`. We only accept **H** (for hourly); **D** (for daily); **W** (for weekly); and **M** (for monthly).
-  `periods`: the length of your future time series wanted to be predicted. 168 hours mean 7 days or one week.
-  `changepoint_prior_scale`: The float value from 0.005 to 0.5. If this value is higher, it is likely to create more complex model to predict complex data.

For the operation **ts_report**, we create a json report of prediction.
`columns`, `ds`, `freq' are similar to the above, but `k` is the integer which tells you how many lowest or highest data points that you want to extract.
For example, if k=3, then we will get 3 data points for the lowest and 3 data points for the highest.


## Output result

```JSON
[
    {
        "variable": "price",
        "report": {
            "lows": {
                "timestamp": [
                    "2023-06-13 00:00:00.000000000",
                    "2023-06-13 01:00:00.000000000",
                    "2023-06-13 02:00:00.000000000"
                ],
                "data": [
                    28961.006210586154,
                    28961.72967118328,
                    28964.175390116823
                ]
            },
            "highs": {
                "timestamp": [
                    "2023-06-15 00:00:00.000000000",
                    "2023-06-15 01:00:00.000000000",
                    "2023-06-15 02:00:00.000000000"
                ],
                "data": [
                    29122.363329864234,
                    29110.4950160422,
                    29100.028115667166
                ]
            }
        },
        "prediction": [
            {
                "date": "2023-06-11 00:00:00.000000000",
                "price": 28825.98417071405,
                "prod_id": 100,
                "prod_name": "Titan"
            },
            {
                "date": "2023-06-11 01:00:00.000000000",
                "price": 28822.05300206496,
                "prod_id": 100,
                "prod_name": "Titan"
            },
            {
                "date": "2023-06-11 02:00:00.000000000",
                "price": 28819.47029639314,
                "prod_id": 100,
                "prod_name": "Titan"
            },
            {
                "date": "2023-06-11 03:00:00.000000000",
                "price": 28820.581461479836,
                "prod_id": 100,
                "prod_name": "Titan"
            },
            {
                "date": "2023-06-11 04:00:00.000000000",
                "price": 28824.87166473548,
                "prod_id": 100,
                "prod_name": "Titan"
            }
        ]
    }
]
```

**Chú thích:**

- `prediction`: Dữ liệu dự đoán cho tương lai, giá trị dự đoán 'price' cho thời gian 'date'.
- `report`: Phần này có hai mục `lows` và `highs`. Vi dụ:
```json
"lows": {
                "timestamp": [
                    "2023-06-13 00:00:00.000000000",
                    "2023-06-13 01:00:00.000000000",
                    "2023-06-13 02:00:00.000000000"
                ],
                "data": [
                    28961.006210586154,
                    28961.72967118328,
                    28964.175390116823
                ]
            }
```
nghĩa là trong quá trình dự báo, chương trình tìm ra 3 thời điểm có giá thấp nhất tương ứng với `timestamp` và `data`. Chẳng hạn tại thời điểm "2023-06-13 00:00:00.000000000" giá thấp nhất là 28961.006210586154.

Tương tự cho `highs`.
