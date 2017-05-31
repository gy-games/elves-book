# 执行及时任务

执行及时任务，因及时任务采用HTTP请求发起与回收结果，所以最长超时时间为HTTP超时时间。

## URI

```
GET /api/v2/rt/exec
```

## 请求及参数

```
/v2/rt/exec?ip={ip}&func={func}&param={param}&timeout={timeout}&proxy={proxy}&app={auth_id}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| ip | 必填 | string | agent ip |
| func | 必填 | string | app方法 |
| param | 选填 | string json | app参数 |
| timeout | 选填 | int | 超时秒数,默认90 |
| proxy | 选填 | string | 自定义代理器 |
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
    "rt_id":"9ad6af3b2e5d4c2f",
    "worker_flag":"1",
    "worker_message":"hello word!",
    "worker_costtime":"74"
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| flag | string enum\("true","false"\) | 接口执行状态 |
| error | string | 接口错误信息 |
| rt\_id | string | 及时任务ID |
| worker\_flag | string enum\("1","0"\) | app-worker执行状态值,1:成功,0失败 |
| worker\_message | string | app-worker执行结果 |
| worker\_costtime | int | app-worker执行时长（毫秒） |



