# DOMINANCE ANALYSIS

This is a 'converter' service which can be use to determine the importance dominance of the features.

## 1. Basic level:

Here is a json sample:
```
JSON
{
    "input": "domin.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "domin_basic",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "TAX", "PTRATIO", "B", "LSTAT"],
            "target": "PRICE",
            "bins": [10, 40]
        }
    }]
}
```

**Parameters**:

- columns: list of input columns.
- target: name of the output column (revenue).
- bins: list of dominance percentages:
For example, bins: [10, 40], that means we split the dominance levels into strong and weak groups: 0%-10% (C); 10%-40% (B); and 40%-100% (A).


**Output result:**
- `NA`: These columns do not have any impact on the revenue (output).
- `positive`: the group of strong impact factors.
- `negative`: the group of bad impact factor. If you get rank A in negative group, that means it is really BAD!

```JSON
{
    "NA": [
        "CRIM",
        "ZN",
        "INDUS",
        "CHAS",
        "NOX",
        "AGE",
        "DIS",
        "RAD",
        "TAX",
        "B"
    ],
    "positive": {
        "rank": "B",
        "data": [
            {
                "column": "RM",
                "dominance_pct": 30.69,
                "impact_factor": 4.51542
            }
        ]
    },
    "negative": {
        "rank": "B",
        "data": [
            {
                "column": "PTRATIO",
                "dominance_pct": 10.91,
                "impact_factor": -0.93072
            },
            {
                "column": "LSTAT",
                "dominance_pct": 31.72,
                "impact_factor": -0.57181
            }
        ]
    }
}
```

## 2. Intermediate 1:

Json sample
```JSON
{
    "input": "domin.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "domin_intermediate1",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "TAX", "PTRATIO", "B", "LSTAT"],
            "target": "PRICE",
            "weak_percentage": 10,
            "strong_percentage": 15
        }
    }]
}
```

**Parameters**:

- columns: list of input columns.
- target: name of the output column (revenue).
- strong_percentage: 15, it means that you want to find the input that gives top 15% of strong impact.
- weak_percentage: 10, it means that you want to find the input that gives bottom 10% badness of the impact.

**Output result:**
```JSON
[
    {
        "rank": "weak",
        "data": [
            {
                "column": "PTRATIO",
                "dominance_pct": 10.91,
                "impact_factor": -0.97365
            }
        ]
    },
    {
        "rank": "strong",
        "data": [
            {
                "column": "RM",
                "dominance_pct": 30.69,
                "impact_factor": 4.22379
            }
        ]
    }
]
```
