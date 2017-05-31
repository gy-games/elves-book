# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

## 修改配置

**./elves-heartbeat/conf/conf.properties**

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

#auth config
auth.mode = supervisor              #权限模式，可选择supervisor或simple模式
auth.localAppInfo={'app1':'1.0.1','app2':'1.0.2'}
```

注：

auth.mode 用于标识supervisor模块是否开启（supervisor：开启，simple：关闭）。

当auth.mode 设置为supervisor，heartbeat返回给elves-agent可以运行的APP的版本信息为 auth.localAppInfo 的数据。

当 auth.mode 设置为simple，heartbeat返回给elves-agent可以运行的APP的版本信息会从elves-supervisor模块获取。

