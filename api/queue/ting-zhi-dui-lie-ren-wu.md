# 提交队列任务

提交队列任务，提交后任务开始执行

## URI

```
GET|POST /api/v2/queue/stop
```

## 请求及参数

```
/v2/queue/stop?queue_id={queue_id}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
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
    "result": {
        "BF0EE718FCC41307": "finish",
        "AF0EE718FCC4130C": "execing",
        "EF0EE718FSEC4130": "stoped"
    }
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| result-&gt;map\(qk\_id:status\) | map\(string , enum\(finish,stoped\)\) | 队列状态,finish:已执行完,execing:指令已发出暂未获得执行结果,stoped:停止成功 |



