# Principal Component Analysis


Principal Component Analysis (PCA) is a statistical technique used for dimensionality reduction while preserving as much variance (information) as possible in a dataset. PCA transforms the original features of a dataset into a smaller set of new features, called principal components, which are uncorrelated and ordered by the amount of variance they explain in the data.


This `statistics` service can be used to reduce the data dimension. There are 3 types of PCA: "pca_raw", "pca_cor", and "pca_cov". The params to call these functions are the same.
	

```JSON
{
  "input": "/tmp/tmpzzcl6ih8",
  "output": "/tmp/tmpklo6llw3",
  "operations": [
    {
      "operator": "pca_raw",
      "options": {
        "top_k": 2,
        "whiten": false,
        "columns": [
          "col_0",
          "col_1",
          "col_2",
          "col_3",
          "col_4",
          "col_5"
        ]
      }
    }
  ]
}

```


***Chú thích:***

- *input*: the input arrow file.
- *output*: the output result.
- *operator*: `pca_raw` the operator to run.
- *options*: a Hashmap that contains 3 keys: top_k, whiten, and columns
  - *top_k*: (Int) number components that we want to get (from 1 to 5).
 - *whiten*: (Boolean) If true, it gets rid of the unit scaling.
 - *columns*: (List of String) multiple columns to reduce the data.

Notice: We can change `operator` to `pca_cor` or `pca_cov` to test all these functions.


