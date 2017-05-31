# Supervisor

supervisor组件负责elves的APP管理与权限管理， 认证模块目前主要向OpenAPI提供服务。

## 修改配置

**./src/main/resource/conf.properties**

```
#zookeeper config
zookeeper.host=192.168.6.117:2181
zookeeper.outTime=10000
zookeeper.root = /elves

#mq config
mq.ip=192.168.6.117
mq.port=5672
mq.user=root
mq.password=root

#FTP adress config
ftp.res.ip=http://192.168.6.117
ftp.res.user=admin
ftp.res.pass=admin
```



