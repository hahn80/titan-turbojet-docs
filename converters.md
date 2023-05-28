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


Bài toán correlation có nhiều level khác nhau. Sẽ đưa ra cách chạy cho từng level (basic, intermediate1,...)

## Bước level: basic (đọc thêm file doc của Cường để biết nó là cái gì)

Hướng dẫn cách gọi qua file params trong file json

File mẫu correlation.json
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
        "operator": "correlation",
        "options":
        {
            "columns": ["RM", "DIS", "PTRATIO", "LSTAT"],
            "target": "PRICE",
            "level": "basic"
        }
    }]
}
```

Chú thích:

- file `housing.arrow` là file arrow input.
- trong `operations` có hai bước chính để chọn: `select` và `correlation`.
- Điểm lưu ý cho `options` của `correlation`. Phần columns là list tên các cột trong dữ liệu cần đưa vào. Biến target là giá trị đầu ra cần tính correlation.
- Tham số `level` dùng để chỉ cấp bậc dịch vụ cần đưa ra output. Đọc file doc của Cường để biết các bậc này.

Các hiển thị kết quả: Sẽ được cập nhật sau.

File mẫu để chạy level=intermediate1 (bậc 2).
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
        "operator": "correlation",
        "options":
        {
            "columns": ["RM", "DIS", "PTRATIO", "LSTAT"],
            "target": "PRICE",
            "level": "intermediate1",
            "bins": [0.3, 0.6, 0.8]
        }
    }]
}
```

Dịch vụ này giống như bước basic, nhưng có thêm tham số `bins`. Tham số này cho phép người dùng phân loại các biến theo phần trăm.
Nếu bins = [0.3, 0.6, 0.8], nghĩa là người dùng muốn tạo các 4 nhóm: Nhóm D: từ 0% - 30% (thấp nhất); nhóm C: từ 30% - 60%; nhóm B: từ 60%-80%; nhóm A: từ 80% - 100% (cao nhất).
Nên đọc file hướng dẫn của Cường để hiểu các phần trăm này có ý nghĩa gì.

Các hiển thị kết quả: Sẽ được cập nhật sau.
