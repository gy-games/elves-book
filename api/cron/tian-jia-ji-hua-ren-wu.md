# 添加计划任务

添加计划任务，计划任务添加后即为开启状态

## URI

```
POST /api/v2/cron/add
```

## 请求及参数

```
/v2/cron/add?ip={ip}&func={func}&param={param}&timeout={timeout}&proxy={proxy}&rule={rule}&app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| ip | 必填 | string | agent ip |
| func | 必填 | string | app方法 |
| param | 选填 | string json | app参数 |
| timeout | 选填 | int | 超时秒数,默认90 |
| proxy | 选填 | string | 自定义代理器 |
| mode | 选填 | string enum\(NP,P\) | 模式，NP不反馈至用户Processor,P反馈至用户Processor，默认为NP |
| rule | 必填 | string | 队列规则\(Quartz语法\) |
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
    "cron_id":"2ad6af3b2e5d4c2e"
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| flag | string enum\("true","false"\) | 接口执行状态 |
| error | string | 接口错误信息 |
| cron\_id | string | 计划任务 ID |



