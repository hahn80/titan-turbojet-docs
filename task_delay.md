# Tracking Delay Service

This is a `transformer` service.

## Delay by tasks
Here is a sample json:
```json
{
    "input": "delay.arrow",
    "output": "output.arrow",
    "operations": [
    {
        "operator": "task_delay",
        "options":
        {
            "task_name": "task_name",
            "appointment": "appointment",
            "completed": "task_completed",
            "standard_hours": "so_gio_xu_ly",
            "late_task": "late_task",
            "delay_time": "late_hours"
        }
    }]
}
```
**Giải thích các tham số:**

- *task_name*: tên cột các task; type: String.
- *appointment*: tên cột thời gian hẹn trả; type: Datetime.
- *completed*: tên cột thời gian hoàn thành; type: Datetime.
- *standard_hours*: tên cột số giờ qui định của Nhà nước; type INT.
- *late_task*: tên cột cho output task bị trễ.
- *delay_time*: tên cột cho output số giờ trễ.

## Kết quả output:
Kết quả là một file arrow với các thành phần số giờ trễ trung bình, số count task trễ, và tỉ task trễ / tổng số task được giao.


