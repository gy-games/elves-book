# 获取APP列表信息

获取此AuthID所对应的APP的基本信息，包含APP指令名称，APP名称、APP版本号以及APP的最后更新时间

## URI

```
GET /api/v2/info/app
```

## 请求及参数

```
/v2/info/app?app={app}&auth_id={auth_id}&timestamp={timestamp}&sign_type={sign_type}&sign={sign}
```

| **字段** | **必填否** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| app | 必填 | string | app名称 |
| auth\_id | 必填 | string | AuthID |
| timestamp | 必填 | int | 当前时间戳 |
| sign\_type | 必填 | string enum\(md5\) | 签名类型 |
| sign | 必填 | string | 签名 |

## 返回结果

```
{
    "app": "apptest",
    "app_name": "测试APP",
    "app_ver": "1.0.0",
    "last_update_time":"2017/05/30 12:42"
}
```

## 返回值说明

| **字段** | **类型** | **注释** |
| :--- | :--- | :--- |
| app | string | app |
| app\_name | string | app名称 |
| app\_ver | string | app版本 |
| last\_update\_time | string string datetime\(\)yyyy-mm-dd jj:ii:sshh | 最后更新时间 |



