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
| 401.1 | Unauthorized Missing Sign Param |  |
| 401.2 | Unauthorized Sign Timeout |  |
| 401.3 | Unauthorized AuthId Is Illegal |  |
| 401.4 | Unauthorized AuthKey Is Illegal |  |
| 401.5 | Unauthorized Sign Error |  |

### 402 Require Module Not Enabled 所需组件未启用

组件依赖相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 402.1 | Require Module \(supervisor\) Not Enabled |  |
| 402.2 | Require Module \(cron\) Not Enabled |  |
| 402.3 | Require Module \(queue\) Not Enabled |  |

### 403 Request Param Illegal 请求参数错误

组件依赖相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
| 403.1 | Request Params \(ip\) Illegal |  |
| 403.2 | Request Params \(param\) Illegal |  |
| 403.3 | Request Params \(app\|\|func\) Illegal |  |
| 403.4 | Request Params \(mode\) Illegal |  |
| 403.5 | Request Params \(cron-rule\) Illegal |  |
| 403.6 | Request Params \(queue\_id\) Illegal |  |

### 410 Scheduler Error

Scheduler组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 411 Cron Error

Cron组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 412 Queue Error

Scheduler组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 413 Supervisor Error

Supervisor组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 414 Heartbeat Error

Heatbeat组件业务相关错误

| **Code** | **Value** | **解释** |
| :--- | :--- | :--- |
|  |  |  |

### 500 Internal ServerError 内部错误

程序内部错误，一般为组件内部抛出了异常，详细信息会追加入错误信息内

