# 获取计划任务列表信息

获取计划任务列表。

## URI

```
GET /api/v2/cron/list
```

## 请求及说明

```
/v2/cron/list?app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| app | 必填 | string | app名称 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回及说明

```
[
    "1ad6af3b2e5d4c2w",
    "2ad6af3b2e5d4c2e",
    "scd6af3b2e5d4c1d"
]
```

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| cron\_id | string | 计划任务ID |



