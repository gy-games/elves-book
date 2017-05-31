# Supervisor

supervisor组件负责elves的APP管理与权限管理， 认证模块目前主要向OpenAPI提供服务。

## 修改配置

**./src/main/resource/conf.properties**

```
#Zookeeper Config
zookeeper.host=10.0.101.1:2181,10.0.101.2:2181,10.0.101.3:2181    #Zookeeper地址
zookeeper.outTime=10000                                           #Zookeeper超时时间
zookeeper.root=/elves                                             #Zookeeper ROOT地址

#MQ Basic Config
mq.ip       = 10.0.101.100                                          #RabbitMQ IP
mq.port     = 5672                                                  #RabbitMQ 端口
mq.user     = admin                                                 #RABBITMQ 账号
mq.password = 1234567890                                            #RABBITMQ 密码                             
mq.exchange = elves                                                 #Exchange 名称


#FTP adress config
ftp.res.ip=http://192.168.6.117
ftp.res.user=admin
ftp.res.pass=admin
```



