# Outliers (IQR)


An **outlier** is a data point that significantly differs from the other observations in a dataset. In the context of **IQR (Interquartile Range)**, an outlier is defined as any data point that lies beyond a certain threshold based on the spread of the middle 50% of the data.

#### IQR Method for Outliers
To identify outliers using the IQR, follow these steps:

1. **Calculate the Quartiles**:
   - **Q1 (First Quartile)**: The median of the lower half of the data (25th percentile).
   - **Q3 (Third Quartile)**: The median of the upper half of the data (75th percentile).
   - **IQR**: The interquartile range is the difference between Q3 and Q1:  
     \[
     \text{IQR} = Q3 - Q1
     \]

2. **Determine the Outlier Thresholds**:
   - **Lower Bound**: \( Q1 - 1.5 \times \text{IQR} \)
   - **Upper Bound**: \( Q3 + 1.5 \times \text{IQR} \)

3. **Outlier Definition**:
   - Any data point that is **below the lower bound** or **above the upper bound** is considered an **outlier**.


This `statistics` service can be used to multiple columns at onces.

Here is a sample json params: In this example we aggregate data for each pair State and City that match a unique pair of CustomerName and Category.
	

```JSON
{
  "input": "/tmp/tmprn1qqh87",
  "output": "/tmp/tmp8wzjyajj",
  "operations": [
    {
      "operator": "iqr_outliers",
      "options": {
        "whisker": 1.2,
        "iterations": 3,
        "columns": [
          "x",
          "y"
        ]
      }
    }
  ]
}
```


***Chú thích:***

- *input*: the input arrow file.
- *output*: the output result.
- *operator*: `iqr_outliers` the operator to run.
- *options*: a Hashmap that contains 3 keys: whisker, iterations, and columns
  - *whisker*: (FLoat) number from 0.5 to 3.5.
 - *iterations*: (Int) The number of times to check outliers.
 - *columns*: (List of String) multiple columns to check outlier at once.

The output will keep the same rows as orginal data.
If columns = ["x", "y"], then the outliers columns must be: ["x_outliers", "y_outliers"]

The values from these columns will tell us whether it is outlier or not. If the value is 0 then it is not an outlier, otherwise it is an outlier. If the value = 2, it means that the outlier is found from the second iteration. This function allows us to run multiple rounds to check outliers.


