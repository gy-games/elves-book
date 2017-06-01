# 获取计划任务详细信息

使用本接口将获取单个计划任务详细信息。

## URI

```
GET /api/v2/cron/detail
```

## 请求及说明

```
/v2/cron/detail?ip={ip}&cron_id={cron_id}&app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| cron\_id | 必填 | string | cron id |
| app | 必填 | string | app名称 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回及说明

```
{
    "falg":"true",
    "error":"",
    "result":{
        "ip"               : "127.0.0.1",
        "app"              : "apptest",
        "func"             : "hello",
        "param"            : "",
        "mode"             : "NP",
        "proxy"            : "",
        "rule"             : "0/5 * * * * ?",
        "last_exec_time"   : "2017/05/30 10:05:01",
        "last_exec_result" :{
            "flag"           : "true",
            "error"          : "",
            "result":{
                "worker_flag"    :"1",
                "worker_message" :"hello word!",
                "worker_costtime":"100"
            }
        }
    }
}
```

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| rule | string | 计划任务规则 |
| last\_exec\_time | string datetime\(yyyy-mm-dd hh:ii:ss\) | 计划任务最后一次执行时间 |
| last\_exec\_result | string json | 计划任务最后一次执行结果 |



