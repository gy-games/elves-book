# 提交队列

提交队列

## URI

```
POST /api/v2/queue/commit
```

## 请求及参数

```
/v2/queue/commit?queue_id={queue_id}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| queue\_id | 必填 | string | 队列ID |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回结果

```
{
    "flag": "true",
    "error": "",
    "result":""
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |




