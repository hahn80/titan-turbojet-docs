# Regression service

This is a multiple regression service for `transformers`.

Here is a json sample
```JSON
{
    "input": "rice.arrow",
    "output": "output.arrow",
    "operations":
    [
        {
            "operator": "regress",
            "options":
            {
                "columns":
                [
                    "area",
                    "perimeter",
                    "major_length",
                    "minor_length",
                    "eccentricity",
                    "convex_area"
                ],
                "target": "result",
                "predict_data":
                {
                    "area":
                    [
                        100,
                        200
                    ],
                    "perimeter":
                    [
                        150,
                        350
                    ],
                    "major_length":
                    [
                        50.5,
                        80.5
                    ],
                    "minor_length":
                    [
                        30.3,
                        20.2
                    ],
                    "eccentricity":
                    [
                        10.2,
                        18.4
                    ],
                    "convex_area":
                    [
                        120,
                        130
                    ]
                }
            }
        }
    ]
}
```

# Giải thích các tham số:

- `columns`: The list of columns as the inputs (type: double).
- `target`: The name of the output value (type double).
- `predict_data`: The map of new input values of each column that we want to predict in the future.


# Output result
The output result will be an ARROW file that contains the predicted values.
