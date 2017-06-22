# \*快速单任务队列

在很多情况下，我们只需要执行一条异步的APP，快速单任务队列既只有一条任务，无需经过创建，添加，提交的过程，接口调用后将直接提交至队列等待执行，此队列的执行结果依然使用获取队列结果借口获取。

## URI

```
GET|POST /api/v2/queue/qksqueue
```

## 请求及参数

```
/api/v2/queue/qksqueue?ip={ip}&app={app}&func={func}&param={param}&timeout={timeout}&proxy={proxy}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| ip | 必填 | string | agent ip |
| func | 必填 | string | app方法 |
| param | 选填 | string json | app参数 |
| timeout | 选填 | int | 超时秒数,默认90 |
| proxy | 选填 | string | 自定义代理器 |
| mode | 选填 | string enum\(NP,P\) | 模式，NP不反馈至用户Processor,P反馈至用户Processor，默认为NP |
| auth\_id | 必填 | string | AuthID |
| app | 必填 | string | app名称 |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回结果

```
{
    "flag": "true",
    "error": "",
    "result":{
        "id":"12d6af3b2e5d4c2e",
        "task_id":"2346af3b2e5d4ca1"
    }
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| id | string | 队列 ID |
| task\_id | string | 单任务ID |



