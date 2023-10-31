# Concat Service

Concasting multiple files into one output arrow.

Here is a sample json param:

```JSON
{
    "input": ["/path1/file1.arrow", "/path2/file2.arrow"],
    "output": "output.arrow",
    "batch_size": 1000,
    "reformat_string": false
}
```

**Chú thích:**

- `input`: the list of files that we want to concat them together.

