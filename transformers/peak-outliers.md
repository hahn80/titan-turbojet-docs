# Outlier for transformers

This is a peak outlier for `transformers` service.

Here is a sample json params:
```json
{
    "input": "data_mini.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "find_peak",
        "options":
        {
            "columns": ["num_feature_1", "num_feature_2"],
            "threshold": 10.0
        }
    }]
}
```
**Chú thích:**

*find_peak* has the following options:
- *columns*: List of columns to check outliers.
- *threshold*: Ngưỡng chênh lệch xem như không có thay đổi. Ví dụ nếu nhảy từ 90 lên 100 rồi về lại 90 thì xem bình thường.
- Kiểu dữ liệu cho `columns` là numerical (int, float).
- Kiểu dữ liệu cho `threshold` là float.


**Output results:**
Nếu outlier có giá trị -1 thì peak nhỏ nhất và 1 là tương ướng với peak lớn nhất.

