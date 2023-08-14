# DOMINANCE ANALYSIS

This is a 'converter' service which can be use to determine the importance dominance of the features.

Here is a json sample:
```
JSON
{
    "input": "domin.arrow",
    "output": "output.json",
    "operations": [
        {
            "operator": "dominance",
            "options": {"columns": ["CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "TAX", "PTRATIO", "B", "LSTAT"], "target": "PRICE"}
        }
    ]
}
```

The params of the dominance serice are simple as other existing services.

**Output result:**

For plotting the result, we can use the template from here:
[https://github.com/dominance-analysis/dominance-analysis]

```JSON
{
    "Dominance Analysis": [
        {
            "Dominance Measures": "Individual Dominance",
            "CRIM": 0.15078,
            "ZN": 0.12992,
            "INDUS": 0.23399,
            "CHAS": 0.03072,
            "NOX": 0.1826,
            "RM": 0.48353,
            "AGE": 0.14209,
            "DIS": 0.06246,
            "RAD": 0.14564,
            "TAX": 0.21953,
            "PTRATIO": 0.25785,
            "B": 0.1112,
            "LSTAT": 0.54415
        },
        {
            "Dominance Measures": "Average Partial Dominance",
            "CRIM": 0.01175,
            "ZN": 0.01299,
            "INDUS": 0.01217,
            "CHAS": 0.01418,
            "NOX": 0.01453,
            "RM": 0.15968,
            "AGE": 0.00859,
            "DIS": 0.03074,
            "RAD": 0.00844,
            "TAX": 0.01291,
            "PTRATIO": 0.05675,
            "B": 0.01226,
            "LSTAT": 0.165
        },
        {
            "Dominance Measures": "Interactional Dominance",
            "CRIM": 0.00569,
            "ZN": 0.00603,
            "INDUS": 6e-05,
            "CHAS": 0.00513,
            "NOX": 0.0114,
            "RM": 0.04381,
            "AGE": 0.0,
            "DIS": 0.02885,
            "RAD": 0.01122,
            "TAX": 0.00567,
            "PTRATIO": 0.02796,
            "B": 0.00634,
            "LSTAT": 0.05644
        },
        {
            "Dominance Measures": "Total Dominance",
            "CRIM": 0.01178,
            "ZN": 0.01302,
            "INDUS": 0.01222,
            "CHAS": 0.01418,
            "NOX": 0.01457,
            "RM": 0.15973,
            "AGE": 0.00862,
            "DIS": 0.03075,
            "RAD": 0.00848,
            "TAX": 0.01296,
            "PTRATIO": 0.05679,
            "B": 0.01228,
            "LSTAT": 0.16507
        },
        {
            "Dominance Measures": "Percentage Relative Importance",
            "CRIM": 2.26,
            "ZN": 2.5,
            "INDUS": 2.35,
            "CHAS": 2.73,
            "NOX": 2.8,
            "RM": 30.69,
            "AGE": 1.66,
            "DIS": 5.91,
            "RAD": 1.63,
            "TAX": 2.49,
            "PTRATIO": 10.91,
            "B": 2.36,
            "LSTAT": 31.72
        },
        {
            "Dominance Measures": "Impact Coefficient",
            "CRIM": 0.0,
            "ZN": 0.0,
            "INDUS": 0.0,
            "CHAS": 0.0,
            "NOX": 0.0,
            "RM": 4.22379,
            "AGE": 0.0,
            "DIS": -0.55193,
            "RAD": 0.0,
            "TAX": 0.0,
            "PTRATIO": -0.97365,
            "B": 0.0,
            "LSTAT": -0.66544
        }
    ]
}
```
