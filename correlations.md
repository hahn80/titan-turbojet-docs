# The instruction to run CORRELATION services


Note that `correlation` is a *converter* service. Use the right settings for Java to call *converter* in *String service*.

## 1. Basic service

Hướng dẫn cách gọi qua file params trong file json

File mẫu basic.json

```JSON
{
    "input": "/mnt/Data/TitanProjects/Polars/src/data.arrow",
    "output": "/mnt/Data/TitanProjects/Polars/src/output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["sum_other_sales_","sum_eu_sales_","sum_jp_sales_","sum_global_sales_","__timestamp","platform"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["sum_eu_sales_","sum_jp_sales_","sum_other_sales_"],
            "target": "sum_global_sales_",
            "level": "basic"
        }
    }]
}
```

**Chú thích:**

- file `data.arrow` là file arrow input.
- trong `operations` có hai bước chính để chọn: `select` và `correlation`.
- Điểm lưu ý cho `options` của `correlation`. Phần columns là list tên các cột trong dữ liệu cần đưa vào. Biến target là giá trị đầu ra cần tính correlation.
- Tham số `level` dùng để chỉ cấp bậc dịch vụ cần đưa ra output. Đọc file doc của Cường để biết các bậc này.

**Cách hiển thị kết quả:**
```json
{
    "target": "sum_global_sales_",
    "correlation": [
        {
            "rank": "A",
            "data": {
                "sum_eu_sales_": 0.9634813584200301,
                "sum_jp_sales_": 0.6478133432993226,
                "sum_other_sales_": 0.8671747166556987
            }
        }
    ]
}
```

Các correlation được phân chia theo *rank*, nếu `rank: A` nghĩa là nó cao nhất, tiếp theo là *B,C,D,F*. Giống như cho điểm học sinh vậy nhé. Điểm A là nhất, còn F thì lo thi lại.
Các hệ số của `data` cho ta biết mức độ tương quan của các biến. Nó giao động từ 0 đến 1.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.


## 2. Low Intermediate service (intermediate1)

File mẫu intermediate1.json
```JSON
{
    "input": "/mnt/Data/TitanProjects/Polars/src/data.arrow",
    "output": "/mnt/Data/TitanProjects/Polars/src/output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["sum_other_sales_","sum_eu_sales_","sum_jp_sales_","sum_global_sales_","__timestamp","platform"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["sum_eu_sales_","sum_jp_sales_","sum_other_sales_"],
            "target": "sum_global_sales_",
            "level": "intermediate1",
            "bins": [0.2, 0.5, 0.7]
        }
    }]
}
```

**Chú thích:**

Tham số `bins` cho phép người dùng phân loại các biến theo phần trăm.
Nếu `bins = [0.2, 0.5, 0.7]`, nghĩa là người dùng muốn tạo các 4 nhóm: Nhóm D: từ 0% - 20% (thấp nhất); nhóm C: từ 20% - 50%; nhóm B: từ 50%-70%; nhóm A: từ 70% - 100% (cao nhất).
Ý nghĩa của phần trăm là khả năng tương quan tính theo phần trăm với biến *target*.

**Cách hiển thị kết quả:**

```JSON
{
    "target": "sum_global_sales_",
    "correlation": [
        {
            "rank": "A",
            "data": {
                "sum_eu_sales_": 0.9634813584200301
            }
        },
        {
            "rank": "C",
            "data": {
                "sum_other_sales_": 0.8671747166556987
            }
        },
        {
            "rank": "D",
            "data": {
                "sum_jp_sales_": 0.6478133432993226
            }
        }
    ]
}
```

- Rank A: Tác động mạnh: Hệ số chiếm trên 70%.
- Rank B: Tác động tương đối mạnh: Hệ số chiếm 50%-70%.
- Rank C: Tác động nhỏ: Hệ số chiếm 20% đến 50%
- Rank D: Tác động không đáng kể: Hệ số chiếm dưới 20%.


## 3. High Intermediate service (intermediate2)

File mẫu intermediate1.json
```JSON
{
    "input": "/mnt/Data/TitanProjects/Polars/src/data.arrow",
    "output": "/mnt/Data/TitanProjects/Polars/src/output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["sum_other_sales_","sum_eu_sales_","sum_jp_sales_","sum_global_sales_","__timestamp","platform"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["sum_eu_sales_","sum_jp_sales_","sum_other_sales_"],
            "target": "sum_global_sales_",
            "level": "intermediate2",
            "lags": [5, 6, 7]
        }
    }]
}
```

**Chú thích:**

- `lags`: là list các độ dài để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.

**Cách hiển thị kết quả:**

```JSON
{
    "target": "sum_global_sales_",
    "correlation": [
        {
            "rank": "B",
            "data": {
                "sum_eu_sales_": 0.35553407192461556,
                "sum_jp_sales_": 0.22770606704601923,
                "sum_other_sales_": 0.37536248438994296
            },
            "shift": {
                "sum_eu_sales_": 5,
                "sum_jp_sales_": 6,
                "sum_other_sales_": 7
            }
        }
    ]
}
```

Trường thông tin *shift* chỉ ra độ lùi của các biến tương ứng. Nếu dữ liệu đưa vào có frequency là giờ (H) thì nó lùi 5 giờ, 6 giờ, 7 giờ.
Các số trong *shift* là thông tin người dùng đưa vào. Dịch vụ *intermediate2* không tìm độ lùi tối ưu cho khách hàng.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.


## 4. Low Advance service (advance1)

File mẫu intermediate1.json
```JSON
{
    "input": "/mnt/Data/TitanProjects/Polars/src/data.arrow",
    "output": "/mnt/Data/TitanProjects/Polars/src/output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["sum_other_sales_","sum_eu_sales_","sum_jp_sales_","sum_global_sales_","__timestamp","platform"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["sum_eu_sales_","sum_jp_sales_","sum_other_sales_"],
            "target": "sum_global_sales_",
            "level": "advance1",
            "lags": [5, 6, 7]
        }
    }]
}
```
**Chú thích:**

- `lags`: là list các độ dài để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.

**Cách hiển thị kết quả:**

```JSON
{
    "target": "sum_global_sales_",
    "correlation": [
        {
            "rank": "A",
            "data": {
                "sum_other_sales_": 0.37536248438994296
            },
            "shift": {
                "sum_other_sales_": 7
            }
        },
        {
            "rank": "C",
            "data": {
                "sum_eu_sales_": 0.35553407192461556
            },
            "shift": {
                "sum_eu_sales_": 5
            }
        },
        {
            "rank": "D",
            "data": {
                "sum_jp_sales_": 0.22770606704601923
            },
            "shift": {
                "sum_jp_sales_": 6
            }
        }
    ]
}
```

Trường thông tin *shift* chỉ ra độ lùi của các biến tương ứng. Nếu dữ liệu đưa vào có frequency là giờ (H) thì nó lùi 5 giờ, 6 giờ, 7 giờ.
Các số trong *shift* là thông tin người dùng đưa vào. Dịch vụ *advance1* không tìm độ lùi tối ưu cho khách hàng.

- Rank A: Tác động mạnh: Hệ số chiếm trên 70%.
- Rank B: Tác động tương đối mạnh: Hệ số chiếm 50%-70%.
- Rank C: Tác động nhỏ: Hệ số chiếm 20% đến 50%
- Rank D: Tác động không đáng kể: Hệ số chiếm dưới 20%.


## 4. High Advance service (advance2)

File mẫu intermediate1.json
```JSON
{
    "input": "data.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "select",
        "options":
        {
            "columns": ["sum_other_sales_","sum_eu_sales_","sum_jp_sales_","sum_global_sales_","__timestamp","platform"]
        }
    },
    {
        "operator": "correlation",
        "options":
        {
            "columns": ["sum_eu_sales_","sum_jp_sales_","sum_other_sales_"],
            "target": "sum_global_sales_",
            "level": "advance2",
            "lags": [10, 8, 9]
        }
    }]
}
```

**Chú thích:**

- `lags`: là list các độ dài (max) để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.
- Thuật toán sẽ tìm ra độ lùi tối ưu để có hệ số correlation cao nhất.

**Cách hiển thị kết quả:**

```JSON
{
    "target": "sum_global_sales_",
    "correlation": [
        {
            "rank": "A",
            "data": {
                "sum_eu_sales_": 0.9628466824176781,
                "sum_jp_sales_": 0.6398213562897531,
                "sum_other_sales_": 0.8654364651425458
            },
            "shift": {
                "sum_eu_sales_": 0,
                "sum_jp_sales_": 0,
                "sum_other_sales_": 0
            }
        }
    ]
}
```

Thuật toán *advance2* sẽ tìm ra độ lùi tối ưu cho các biến tương ứng. Với kết quả ở trên, thuật toán chỉ cho người dùng thấy rằng để có hệ số tương quan cao nhất, độ lùi của các biến đều bằng 0.
Trong nhiều trường hợp tổng quát, các số lùi này sẽ khác nhau.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.
