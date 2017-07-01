# 获取Agent列表信息

获取此AuthID对应的APP可运行的AGENT的列表信息，包含AGENT别称，AGENT IP。

## URI

```
GET|POST /api/v2/info/agents
```

## 请求及说明

```
/v2/info/agents?auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回及说明

```
{
    "flag":"true",
    "error":"",
    "result":[
        "127.0.0.1",
        "192.168.0.1",
        "172.32.0.1"
    ]
}
```

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| result | list&lt;string&gt; | AGENT IP 集合 |



