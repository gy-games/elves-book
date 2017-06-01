# 获取队列任务结果

提交队列任务，提交后任务开始执行

## URI

```
GET /api/v2/queue/result
```

## 请求及参数

```
/v2/queue/result?queue_id={queue_id}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
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
    "task_result": {
        "BF0EE718FCC41307": {
            "status":"finish",
            "depend_task_id":"",
            "flag": "true",
            "error": "",
            "task_id": "BF0EE718FCC41307",
            "worker_flag": "1",
            "worker_message": "hello word!",
            "worker_costtime": "74"
        },
        "AF0EE718FCC41301": {
            "status":"execing",
            "depend_task_id":"BF0EE718FCC41307",
            "flag": "",
            "error": "",
            "task_id": "",
            "worker_flag": "",
            "worker_message": "",
            "worker_costtime": ""
        },
        "CF0EE718FCC41302": {
            "status":"pendding",
            "depend_task_id":"BF0EE718FCC41307",
            "flag": "",
            "error": "",
            "task_id": "",
            "worker_flag": "",
            "worker_message": "word!",
            "worker_costtime": ""
        }
    }
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| flag | string enum\("true","false"\) | 接口执行状态 |
| error | string | 接口错误信息 |
| status | string json enum\(finish,stoped\) | 队列状态finish:已执行完,execing:指令已发出暂未获得执行结果,pendding:等待执行 |



