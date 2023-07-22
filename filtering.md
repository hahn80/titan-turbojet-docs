# FILTERING SERVICE

This service to deal with the WHERE statement in SQL

Here is a sample json param:

```JSON
{
    "input": "left.arrow",
    "output": "output.arrow",
    "batch_size": 1000,
    "reformat_string": false,
    "operations": [
    {
        "operator": "filtering",
        "options":
        {
            "where": "cr7.subject like '^Thông\\stư' and cr7.id in (713, 718, 1171)",
            "type_format": {"cr7.subject": "STRING", "cr7.id": "INT"}
        }
    }]
}
```

**Chú thích:**

Tham số `where` có kiểu là string tương tự như WHERE trong SQL. Nó hỗ trợ các tính năng sau:

- "where": "`name1 BETWEEN A AND B`"
- "where": "`name1 IN (A, B)`"
- "where": "`name1 = Null`"
- "where": "`name1 != Null`"
- "where": "`name1 = ABC`"
- "where": "`name1 >= 10`"
- "where": "`name1 <= 20`"
- "where": "`name1 LIKE '^[A,a]\sB{3}'`" -> Regex support.
