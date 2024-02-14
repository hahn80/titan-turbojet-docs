# Dominance Transformers

This is a `transformers` service to determine the dominance importances. This is not the same as dominance service for `converter`.

## 1. Level a:

Here is a json sample:
```
JSON
{
    "input": "auto.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "dominance_a",
        "options":
        {
            "columns": [
                "mpg",
                "rep78",
                "headroom",
                "trunk",
                "weight",
                "length",
                "turn",
                "displacement",
                "gear_ratio"
            ],
            "target": "price",
            "impact_percentage": [5, 20],
            "confidence_level": 0.75
        }
    }]
}

```

**Parameters**:

- columns: list of input columns.
- target: name of the output column (revenue).
- impact_percentage: list of dominance percentages:
For example, impact_percentage: [5, 20], that means we split the dominance levels into 5 groups: 0%-10% (0 = weak positive); 10%-40% (1 = moderate positive); and 40%-100% (2 = strong positive) or 10%-40% (-1 = moderate negative); and 40%-100% (-2 = strong negative).
- `confidence_level`: a number from 0.5 to 0.95 (this number represent 50% to 95% confidence)


**Output result:** The output will be an arrow file.


## 2. Level b:

Json sample
```JSON
{
    "input": "auto.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "dominance_b",
        "options":
        {
            "columns": [
                "mpg",
                "rep78",
                "headroom",
                "trunk",
                "weight",
                "length",
                "turn",
                "displacement",
                "gear_ratio"
            ],
            "target": "price",
            "impact_percentage": [5, 20],
            "confidence_level": 0.75,
            "shifts": [
                1,
                2,
                3,
                2,
                1,
                2,
                4,
                2,
                3
            ]
        }
    }]
}
```

**Parameters**:

- `shifts`: the shift units for each columns (integer) and must have same length as `columns`.

**Output result:** The output will be an arrow file
