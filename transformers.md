# How to call functions with transformer service?

Transformers will contain serveral algorithms to run ML tasks.

## Clustering

Here is the sample of json file to run the service:
```json
{
    "input": "iris.arrow",
    "output": "output.arrow",
    "operations": [
        {
            "operator": "select",
            "options": {"columns": ["x1", "x2", "x4"]}
        },
        {
            "operator": "clustering",
            "options": {"cluster_name": "hac", "columns": ["x1", "x2"], "n_clusters": 3}
        }
    ]
}
```
We can create the json object in Java, convert it to HashMap and pass it directly to Python using Py4J.
Let me explain the information in the above json params.
`"input": "iris.arrow"` is the input of arrow format. We also work with arrow!
`"output": "output.arrow"` is the output file of the result. Note that the transformers always gives the output in the arrow format, so the output file should be in the arrow format.

`"operations": []` will contain a list of operations. We can perform multiple operations as you want!
Each entry of operations is a dictionary, for example
```json
{
            "operator": "clustering",
            "options": {"cluster_name": "hac", "columns": ["x1", "x2"], "n_clusters": 3}
        }
```
It tells us that we call function `clustering` passing params `"options": {"cluster_name": "hac", "columns": ["x1", "x2"], "n_clusters": 3}` to this function.

The only thing we need to change is the `"cluster_name"`. There are the following clustering names you can call:

- hac: Hierarchical Agglomerate Clustering. For example `"cluster_name": "hac"`.
- kmean: K-means Clustering. For example `"cluster_name": "kmean"`.
- spectral: Spectral Clustering
- gmm: Gaussian Mixture Clustering
- bgm: Bayesian Mixture Clustering
- meanshift: MeanShift Clustering
- affinity: Affinity Propagation Clustering
- bdscan: DBSCAN Clustering
- optics: Optics Clustering

## Imputer
This function will fill the missing values for data preprocessing.
Here is the sample of json file to run the service:
```json
{
    "input": "outlier.arrow",
    "output": "output.arrow",
    "operations": [
        {
            "operator": "select",
            "options": {"columns": ["x1", "x2", "x3"]}
        }
        {
            "operator": "imputer",
            "options": {"columns": ["x3"], "method": "simple", "strategy": "mean"}
        },
        {
            "operator": "imputer",
            "options": {"columns": ["x2"], "method": "interpolate"}
        },
        {
            "operator": "imputer",
            "options": {"columns": ["x1"], "method": "knn", "n_neighbors": 5}
        },
        {
            "operator": "imputer",
            "options": {"columns": ["x2"], "method": "bayesian", "max_iter": 10}
        }
    ]
}

```
The only thing we want to change is the `"method"`. There are the following methods
- simple: A simpple imputation with different strategies: ‘forward’, ‘backward’, ‘min’, ‘max’, ‘mean’, ‘zero’, ‘one
- interpolate: Fill nulls with linear interpolation over missing values.
- knn: Impute the missing values using k-Nearest Neighbors
- bayesian: Multivariate imputer using BayesianRidge estimator.
