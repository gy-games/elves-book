# 签名认证

接口签名是对请求的所有参数进行一个摘要计算以防止伪造请求的方法，对于以下的请求：

```
http://host:port/foo/bar?key2=value2&key1=value1&timestamp=1234567890
```

经过签名计算后的请求如下：

```
http://host:port/foo/bar?key2=value2&key1=value1&auth_id=x&timestamp=1234567890&sign_type=MD5&sign=fb316e29172065a840090ddc759f4dff
```

**签名的计算方法为：                    
**

1. 生成待签名字符串 将 Uri 的 Path 部分（在本例中为："/foo/bar"），与接口指定的 Uri 参数对（通常为除 sign 和 sign\_type 外的所有参数，则在本例中为"key2=value2", "key1=value1", "timestamp=1234567890"）经过字母顺序排序后以“&”连接的字符串通过“?”进行拼接进行拼接，生成最终带签名字符串，即本例中结果（signString）为：

   ```
   /foo/bar?key1=value1&key2=value2&timestamp=1234567890
   ```

2. 对待签名字符串进行签名计算 使用在 sign\_type 中指定的签名算法计算后，赋值给 sign 参数。在进行签名计算时，应使用待签名字符串的UTF-8编码字节。

如：在使用 MD5 算法进行签名时，待签名字符串后应当拼接双方约定好的私钥，之后对拼接后的待签名字符串进行摘要计算（MD5\(signString+key\)），即本例中若密钥为“testkey”，待计算MD5的字符串为：

```
   /foo/bar?key1=value1&key2=value2&timestamp=1234567890testkey
```

将计算出来的签名 sign 及 sign\_type 与原参数拼接后，最后生成的URI则将如文首所示；

1. 其他注意事项 对于在传递中进行了 url 编码的参数，在拼接待签名字符串时应使用的是未经编码的原值。 如果接口要求时间戳则应为 Unix 标准时间戳，且服务端应当对请求有效时间做出约定。

2. 签名算法： 1. MD5 在使用 MD5 进行签名时，待签名字符串后应当拼接双方约定好的私钥，之后对拼接后的待签名字符串进行摘要计算。

**若开启了签名认证，在所有请求必须增加以下公共参数**

| **字段** | **是否必填** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| auth\_id | 必填 | int | 由Supervisor生成的AuthID或OpenAPI内置的AuthID |
| timestamp | 必填 | int | 当前的时间戳 |
| sign\_type | 必填 | string | 类型名称（MD5） |
| sign | 必填 | string | 签名值 |



