# 返回值与码表

# Flag Error Code

### 401 Unauthorized 权限错误

权限认证相关错误

| **Code** | **返回值** | **解释** |
| :--- | :--- | :--- |
| 401.1 | Unauthorized Missing Sign Param |  |
| 401.2 | Unauthorized Sign Timeout |  |
| 401.3 | Unauthorized AuthId Is Illegal |  |

### 402 Require Module Not Enabled 所需组件未启用

组件依赖相关错误

| **Code** | **返回值** | **解释** |
| :--- | :--- | :--- |
| 402.1 | Require Module \(supervisor\) Not Enabled |  |
| 402.2 | Require Module \(cron\) Not Enabled |  |
| 402.3 | Require Module \(queue\) Not Enabled |  |

### 403 Request Param Illegal 请求参数错误

组件依赖相关错误

| **Code** | **返回值** | **解释** |
| :--- | :--- | :--- |
| 403.1 | Request Params \(ip\) Illegal |  |
| 403.2 | 403.2Request Params \(param\) Illegal |  |
| 403.3 | Request Params \(app\|\|func\) Illegal |  |

### 500 Internal ServerError 内部错误

程序内部错误，一般为组件内部抛出了异常，详细信息会追加入错误信息内

