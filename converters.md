# How to run the converters service?
This documentation will give you the instruction to run this service.

## Dominance Analysis
Here is a sample json to call the function
```json
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
This is a very expensive computation algorithm. There is some restriction on the `columns` param. The numbers of columns must be at least 3 and at most 15.
We can also ignore completely the `options` to let the `dominance` figure it out itself.
For example
```json
{
    "input": "domin.arrow",
    "output": "output.json",
    "operations": [
        {
            "operator": "dominance"
        }
    ]
}
```

## Linear Regression
This function will perform the linear regression with all statistics related to the model.
Here is a sample json:
```json
{
    "input": "housing.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "PTRATIO", "LSTAT", "PRICE"]
        }
    },
    {
        "operator": "linear_reg",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "RM", "AGE", "DIS", "PTRATIO", "LSTAT"],
            "target": "PRICE"
        }
    }]
}
```
The first operation:
```json
    {
        "operator": "select",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "PTRATIO", "LSTAT", "PRICE"]
        }
    }
```
to select the columns from a big arrow file.
Next, we perform the linear regression
```json
{
        "operator": "linear_reg",
        "options":
        {
            "columns": ["CRIM", "ZN", "INDUS", "CHAS", "RM", "AGE", "DIS", "PTRATIO", "LSTAT"],
            "target": "PRICE"
        }
    }
```
Note that `"columns": ["CRIM", "ZN", "INDUS", "CHAS", "RM", "AGE", "DIS", "PTRATIO", "LSTAT"]` is the list of predictors, and `"target": "PRICE"` is the dependent variable to be predicted.
The output result can be viewed using website: http://json2table.com


## Correlation
The function will calculate the correlation coefficients of each column vs the *target*.

The input arrow should have the form

|x1|x2|x3|target|
|---|---|---|---|
|10|-2|5|-2|
|2|4|6|4|
|.|.|.|.|
|100|30|45|20|

Here is a sample json:
```json
{
    "input": "input.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["x1", "x2", "x3", "target"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["x1", "x2"],
            "target": "target"
        }
    }]
}
```
