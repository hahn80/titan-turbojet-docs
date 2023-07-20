# FILTERING SERVICE

This service to deal with the WHERE statement in SQL

Here is a sample json param:

```JSON
{
    "input": "sql.arrow",
    "output": "output.arrow",
    "batch_size": 4000,
    "reformat_string": true,
    "operations": [
    {
        "operator": "filtering",
        "options":
        {
            "where": "CustomerID between 10 and 13"
        }
    }]
}
```

**Chú thích:**

Tham số `where` có kiểu là string tương tự như WHERE trong SQL. Nó hỗ trợ các tính năng sau:

- "where": "`name1 BETWEEN A AND B`"
- "where": "`name1 IN [A, B]`"
- "where": "`name1 = Null`"
- "where": "`name1 != Null`"
- "where": "`name1 = ABC`"
- "where": "`name1 >= 10`"
- "where": "`name1 <= 20`"
- "where": "`name1 LIKE '^[A,a]\sB{3}'`" -> Regex support.
