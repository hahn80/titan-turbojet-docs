# Casting Service

Trong một số trường hợp, khi groupby và tính toán, các kết quả sẽ cho dtype khác với mong muốn. Dịch vụ này sẽ cho phép người dùng chỉnh lại dtype cho output.

Here is a sample json param:

```JSON
{
    "output": "output.arrow",
    "input": "data.arrow",
    "batch_size": 32000,
    "reformat_string": true,
    "operations": [
    {
        "operator": "groupby",
        "options":
        {
            "by": ["tbl.city_dest", "tbl.airline"],
            "aggregates":
            {
                "tbl.city_dest": [
                {
                    "func": "count"
                },
                {
                    "func": "n_unique"
                }]
            }
        }
    },
    {
        "operator": "rename",
        "options":
        {
            "mapping":
            {
                "count(tbl.city_dest)": "so_luong_city_dest",
                "n_unique(tbl.city_dest)": "u_city_dest",
                "tbl.airline": "airline",
                "tbl.city_dest": "city_dest"
            }
        }
    },
    {
        "operator": "casting",
        "options":
        {
            "mapping":
            {
                "so_luong_city_dest": "INT",
                "u_city_dest": "DOUBLE"
            }
        }
    }]
}
```

**Chú thích:**

Tham số `cast_map` là hashmap với key: value là tên column trong dataframe và dtype cho output. Chúng ta có thể dùng các kiểu sau: `INT`; `LONG`; `DOUBLE`.

