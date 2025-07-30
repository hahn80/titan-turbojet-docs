# Transpose Service

This `transpose` service can be used with the following json sample:
```JAVA
String service = "transformers";
```

1. Trường hợp Dataframe đã có một cột để làm new header columns:

Ví dụ: Xét dữ liệu như sau:

| abc_ct | # n2023 | # n2024 | # n2025 |
|--------|---------|---------|---------|
| CT4    | 4       | 13      | 103     |
| CT1    | 1       | 10      | 100     |
| CT5    | 5       | 14      | 104     |
| CT3    | 3       | 12      | 102     |
| CT2    | 2       | 11      | 101     |

Trong trường hợp này, chúng ta sẽ dùng cột `abc_ct` để lấy các giá trị theo thứ tự "CT4", "CT1", "CT5", "CT3", "CT2" để đặt tên cho column mới.

Chúng ta sẽ dùng params sau:

```json
{
  "input": "/tmp/tmpsb2mvada",
  "output": "/tmp/tmpkny3w2rx",
  "operations": [
    {
      "operator": "transpose",
      "options": {
        "include_header": true,
        "header_name": "new_headers",
        "header_columns": "abc_ct"
      }
    }
  ]
}
```

Giải thích tham số:

- *include_header* (Bool) true: Include the header
- *header_name* (String): Name of header column
- *header_columns* (String): Tên cột để lấy giá trị cho các cột mới trong kết quả output.

Kết quả output như sau:

| new_headers | CT4 | CT1 | CT5 | CT3 | CT2 |
|-------------|-----|-----|-----|-----|-----|
| # n2023     | 4   | 1   | 5   | 3   | 2   |
| # n2024     | 13  | 10  | 14  | 12  | 11  |
| # n2025     | 103 | 100 | 104 | 102 | 101 |


2. Trường hợp Dataframe không có cột để lấy giá trị cho tên column trong kết quả output

Here is the input Dataframe:

| # n2023 | # n2024 | # n2025 |
|---------|---------|---------|
| 4       | 13      | 103     |
| 1       | 10      | 100     |
| 5       | 14      | 104     |
| 3       | 12      | 102     |
| 2       | 11      | 101     |


In this case, the dataframe has 5 rows, so the transposed output must have 5 columns for the values and one extra column for the headers.

Sample json params:

```json
{
  "input": "/tmp/tmp83aje1ay",
  "output": "/tmp/tmpns4mh70c",
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
    }
  ]
}
```

Giải thích tham số:

- *include_header* (Bool) true: Include the header
- *header_name* (String): Name of header column
- *header_columns* (String List): Since the input dataframe has 5 rows, the output must have 5 column names for the values.

Kết quả output như sau:

| new_headers | col_1 | col_2 | col_3 | col_4 | col_5 |
|-------------|-------|-------|-------|-------|-------|
| # n2023     | 4     | 1     | 5     | 3     | 2     |
| # n2024     | 13    | 10    | 14    | 12    | 11    |
| # n2025     | 103   | 100   | 104   | 102   | 101   |


