## Scoring DataSet - Transformers


This is a `transformers` service and here is a sample json params:

```JSON
{
    "input": "data_mini.arrow",
    "output": "output.arrow",
    "batch_size": 1000,
    "operations": [
    {
        "operator": "scoring_dataset",
        "options":
        {
            "groups":
            {
                "num":[
                    "num_feature_1",
                    "num_feature_2",
                    "num_feature_3",
                    "num_feature_4",
                    "num_feature_5",
                    "num_feature_6",
                    "num_feature_7"],
                "cat":[
                    "cat_feature_1", 
                    "cat_feature_2", 
                    "cat_feature_3", 
                    "cat_feature_4", 
                    "cat_feature_5", 
                    "cat_feature_6", 
                    "cat_feature_7", 
                    "cat_feature_8"
                ]
            },
            "report": [
                "score_duplicate", 
                "score_missing_values", 
                "score_distribution", 
                "score_frequency"
            ]
        }
    }]
}
```

**Chú thích:**

- *groups* -> Danh sách các columns được phân chia làm 2 loại: `num` là list các numerical columns, và `cat` là list các categorical columns.
-  *report* -> Hiện tại đang hỗ trợ 4 report là `duplicate`, `missing_values`, `distribution`, và `frequency`. Kết quả sẽ tự động thêm phần total score.



