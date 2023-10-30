# Business Performance Service

This service is for Titan Finance. Here is a json sample:

```json
{
    "input": "input.arrow",
    "output": "output.arrow",
    "batch_size": 1000,
    "operations": [
    {
        "operator": "business_performance",
        "options":
        {
            "ds": "DATE",
            "groups":
            {
                "actual_cost":
                {
                    "columns": ["C1_NguoiLaoDong", "C2_NguyenVatLieu", "C3_ChiPhiKhac"],
                    "name": "tong_chi_thuc_te"
                },
                "projected_cost":
                {
                    "columns": [
                        "TC1_ChiTieuNguoiLaoDong",
                        "TC2_ChiTieuNguyenVatLieu",
                        "TC3_ChiTieuChiPhiKhac"
                    ],
                    "name": "tong_chi_du_kien"
                },
                "actual_revenue":
                {
                    "columns": [
                        "R1_DoanhThuBanHang",
                        "R2_DoanhThuTaiChinh",
                        "R3_DoanhThuKhac"
                    ],
                    "name": "tong_thu_thuc_te"
                },
                "projected_revenue":
                {
                    "columns": [
                        "TR1_ChiTieuDoanhThuBanHang",
                        "TR2_ChiTieuDoanhThuTaiChinh",
                        "TR3_ChiTieuDoanhThuKhac"
                    ],
                    "name": "tong_thu_du_kien"
                }
            },
            "alias":
            {
                "projected_profit": "loi_nhuan_du_kien",
                "actual_profit": "loi_nhuan_thuc_te"
            },
            "report": [
                "total_by_time",
                "percent_by_total",
                "percent_by_kpi",
                "percent_change_month",
                "percent_change_year"
            ],
            "ds_filter":
            {
                "year": 2022,
                "month": 3
            },
            "agg_fn": "sum"
        }
    }]
}
```

## Params Explanation:

For operation **business_performance**, we need to understand the following things:
- `ds`: The name of the datetime column.
- `groups`: The list of 4 main sources: `actual_cost`; `projected_cost`; `actual_revenue`; and `projected_revenue`.
	- `actual_cost`: This is the actual cost of your business model. It includes `columns` the list of columns for autual cost. **We only accept datatype double for these columns**.
	- `name`: The name for the resulting column when we compute the sum of all actual cost.
- `alias`: If available, it allows you to change the name of `actual_profit` and `projected_profit`.
- `report`: The list of each step in Problem #8 (Check Cuong's document to understand it!). The longest list of report includes: `total_by_time`; `percent_by_total`; `percent_by_kpi`; `percent_change_month`; `percent_change_year`. You can call some of these of you want to.
- `ds_filter`: The filter for datetime. You can include the year, and month. Note that you cannot include quarter and month in once ds_filter.
- `agg_fn`: The default function is `sum`. You don't have change it if you don't know how it works!



## Output result
Each row of the output will be the result of each report (or step) in the `report` paramater.


