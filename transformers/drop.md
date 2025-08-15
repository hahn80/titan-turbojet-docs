# Drop some columns

This `transpose` service can be used with the following json sample:
```JAVA
String service = "transformers";
```

Chúng ta có thể drop các columns ngay từ đầu, trong giữa hoặc cuối cùng của quá trình tính toán. Tham số cho dịch này như sau:

```json
{
            "operator": "drop",
            "options": {
                "columns": [
                    "col_1",
                    "col_2",
                    "col_3",
                ],
            },
        }
```

Ví dụ sau đây sẽ giúp chúng ta kết hợp hàm transpose và drop cùng một lúc. Tuy nhiên drop có thể chạy riêng một cách độc lập.


Tham số mẫu để chạy transpose và drop:

```json
{
  "input": "/tmp/tmpfy7j3ead",
  "output": "/tmp/tmp3tffcgln",
  "operations": [
    {
      "operator": "transpose",
      "options": {
        "include_header": true,
        "header_name": "new_headers",
        "header_columns": [
          "col_1",
          "col_2",
          "col_3",
          "col_4",
          "col_5"
        ]
      }
    },
    {
      "operator": "drop",
      "options": {
        "columns": [
          "col_1",
          "col_2",
          "col_3"
        ]
      }
    }
  ]
}
```

Kết quả output sẽ như thế này:

| new_headers | col_4 | col_5 |
|-------------|-------|-------|
| # n2023     | 3     | 2     |
| # n2024     | 12    | 11    |
| # n2025     | 102   | 101   |



