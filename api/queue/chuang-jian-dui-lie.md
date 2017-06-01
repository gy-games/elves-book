# 创建队列

创建空队列，等待添加任务。

## URI

```
GET /api/v2/queue/create
```

## 请求及参数

```
/v2/queue/create?app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| app | 必填 | string | app名称 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回结果

```
{
    "flag": "true",
    "error": "",
    "result":{
        "queue_id":"12d6af3b2e5d4c2e"
    }
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| queue\_id | string | 队列ID |



