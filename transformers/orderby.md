# ORDERBY SERVICE

This is a transformer service.

Here is a sample json params:

```JSON
{
    "input": "left.arrow",
    "output": "output.arrow",
    "batch_size": 4000,
    "reformat_string": true,
    "operations": [
    {
        "operator": "orderby",
        "options":
        {
            "by": ["cr7.id", "cr7.category"],
            "descending": [true, false]
        }
    }]
}
```

**Chú thích:**

Tham số `by` có kiểu là string hoặc list of strings. Ví dụ: `"by": "name"` hoặc `"by": ["name1", "name2"]`.

Tham số `descending` có kiểu bool hoặc list of bools. Ví dụ: `"descending": true` hoặc `"descending": [true, false]`.
Nếu `descending` là list thì phải cùng length với tham số `by`.
