# 添加任务

添加任务项，任务项创建后需要将任务项组装成队列提交执行。

## URI

```
POST /api/v2/queue/addtask
```

## 请求及参数

```
/v2/queue/addtask?queue_id={queue_id}&ip={ip}&func={func}&param={param}&timeout={timeout}&proxy={proxy}&depend_task_id={depend_task_id}&app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| queue\_id | 必填 | string | 队列ID |
| ip | 必填 | string | agent ip |
| func | 必填 | string | app方法 |
| param | 选填 | string json | app参数 |
| timeout | 选填 | int | 超时秒数,默认90 |
| proxy | 选填 | string | 自定义代理器 |
| mode | 选填 | string enum\(NP,P\) | 模式，NP不反馈至用户Processor,P反馈至用户Processor，默认为NP |
| depend\_task\_id | 选填 | string | 依赖的任务项ID |
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
    "qk_id":"12d6af3b2e5d4c2e"
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| flag | string enum\("true","false"\) | 接口执行状态 |
| error | string | 接口错误信息 |
| task\_id | string | 任务 ID |



