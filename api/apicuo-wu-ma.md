# 返回值与码表

接口采用统一的返回值码与统一的返回值格式，接口的返回值均采用Json格式。

```
{
    "flag":"true/false",     #返回值状态，继标识程序状态又标识业务状态
    "error":"",              #错误内容，返回值格式为：[Code]Value
    "result":[Object]        #返回值内容
}
```

# Flag Error Code

### 401 Unauthorized 权限错误

权限认证相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 401.1 | Unauthorized Missing Sign Param | 权限认证参数有误 |
| 401.2 | Unauthorized AuthId Is Illegal | AuthID时间超时 |
| 401.3 | Unauthorized SignType Is Illegal | 签名类型不合法 |
| 401.4 | Unauthorized Sign Length Is Illegal | 签名长度不合法 |
| 401.5 | Unauthorized Sign Error | 签名错误 |
| 401.6 | Unauthorized AuthId Permission Denied | AuthID没有操作权限 |
| 401.7 | Unauthorized Sign Timeout | 时间戳超时 |

### 402 Require Module Not Enabled 所需组件未启用

组件依赖相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 402.1 | Require Module \(supervisor\) Not Enabled | 依赖的组件supervisor未启用 |
| 402.2 | Require Module \(cron\) Not Enabled | 依赖的组件cron未启用 |
| 402.3 | Require Module \(queue\) Not Enabled | 依赖的组件queue未启用 |

### 403 Request Param Illegal 请求参数错误

组件依赖相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 403.1 | Request Params \(ip\) Illegal | IP参数不合法 |
| 403.2 | Request Params \(param\) Illegal | param参数不合法 |
| 403.3 | Request Params \(app\|\|func\) Illegal | app或func不合法 |
| 403.4 | Request Params \(mode\) Illegal | mode参数不合法 |
| 403.5 | Request Params \(cron-rule\) Illegal | cron-rule参数 |
| 403.6 | Request Params \(queue\_id\) Illegal | queue\_id参数不合法 |

### 410 Scheduler Error

Scheduler组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 410.1 | Send Job To Agent Fail | 发送任务到Agent失败 |

### 411 Cron Error

Cron组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 411.1 | Not Find Cron Job To Operate | 未找到CronJob进行操作 |

### 412 Queue Error

Scheduler组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 413 Supervisor Error

Supervisor组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 413.1 | AuthId Not Found | 未找到此AuthID |

### 414 Heartbeat Error

Heatbeat组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 414.1 | Agent Info Not Found | 未找到Agent信息 |

### 500 Internal ServerError 内部错误

程序内部错误，一般为组件内部抛出了异常，详细信息会追加入错误信息内

