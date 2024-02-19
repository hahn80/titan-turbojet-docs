# Math calculation service

This is a collection of simple math computations service for `transformers`.

Here is a json sample
```JSON
{
    "input": "rice.arrow",
    "output": "output.arrow",
    "operations":
    [
        {
            "operator": "math",
            "options":
            {
                "columns":
                [
                    "area",
                    "perimeter",
                    "convex_area"
                ],
                "funcs":
                [
                    "log",
                    "square",
                    "z_score"
                ]
            }
        }
    ]
}
```

# Giải thích các tham số:

- `columns`: The list of columns as the inputs (type: double, float, int).
- `funcs`: The list of functions to compute for each column, must have same length as columns. We support the following functions:
	-  "exp"
	-  "log"
	-  "log10"
	-  "sqrt"
	-  "square"
	-  "log1p"
	-  "z_score"


# Output result
The output result will be an ARROW file.
