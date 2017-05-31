# 停止计划任务

停止计划任务

## URI

```
POST /api/v2/cron/stop
```

## 请求及参数

```
/v2/cron/stop?cron_id={cron_id}&app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| cron\_id | 必填 | string | 计划任务ID |
| app | 必填 | string | app名称 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回结果

```
{
    "flag": "true",
    "error": ""
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| flag | string enum\("true","false"\) | 接口执行状态,true即为执行成功 |
| error | string | 接口错误信息 |



