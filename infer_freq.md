# Get inferred frequency (arrow)

Note that, we need to run outlier as `converter` service to get the output as JSON format.

Here is a sample json:
```json
{
    "input": "seasonal.arrow",
    "output": "output.json",
    "operations": [
    {
        "operator": "infer_freq",
        "options":
        {
            "ds": "timestamp",
			"strategy": "mode"
        }
    }]
}
```
**Chú thích các tham số:**

- *ds*: name of the datetime column. Type: String
- *strategy*: has two options: `mode` or `min`. Type: String
	+ `mode`: chọn frequecy theo số đông xảy ra trong data.
	+ `min`: chọn frequency theo số biến động thời gian nhỏ nhất.


## Output
Sample of the result:
```json
{
    "unit": "day",
    "size": 1,
    "freq": "D"
}
```
`freq` trong output này được dùng để đưa vào chạy thuật toán time series với forecast_by.
Nếu data không có frequency thống nhất, chương trình sẽ trả về null json
```json
{
    "unit": null,
    "size": null,
    "freq": null
}
```
