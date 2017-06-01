# 获取AGENT详细信息

使用本接口将获取单个AGENT的详细信息，包含其IP，别称，上线时间，计划任务列表，此接口依赖于Heartbeat，若未启用heartbeat则此接口不可用，计划任务列表部分信息依赖于cron组件，若未启用cron组件则计划任务列表不可用。

## URI

```
GET /api/v2/info/agents/detail
```

## 请求及说明

```
/v2/info/agents/detail?ip={ip}&showcron={showcron}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| ip | 必填 | string | AGENT IP |
| showcron | 选填 | string enum\(false,true\) | 是否显示cron信息，默认为false，此选项依赖于cron组件 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回及说明

```
{
    "ip"           : "127.0.0.1",
    "asset"        : "localhost",
    "last_hb_time"  : "2017/05/30 10:05:01",
    "cron_list"    : ["A49B1E8FA1B43BC5","B39B1E8FA1B43BA4"]
}
```

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| ip | string | AGENT IP（全局唯一） |
| asset | string | AGENT别称 |
| last\_hb\_time | string datetime\(yyyy-mm-dd hh:ii:ss\) | AGENT最后通讯时间 |
| cron\_list | string list | cron ID列表 |



