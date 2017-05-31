# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

## 修改配置

**./elves-heartbeat/conf/conf.properties**

```
#zookeeper config
zookeeper.host=192.168.6.117
zookeeper.outTime=10000
zookeeper.root= /elves
zookeeper.heartbeat.port=11102

#mq config
mq.ip=192.168.6.117
mq.port=5672
mq.user=root
mq.password=root

#other config
other.switch=0
other.localAppInfo={'app1':'1.0.1','app2':'1.0.2'}
```

注：

other.switch 用于标识supervisor模块是否开启（0：开启，1：关闭）。 

当 other.switch设置为1，heartbeat返回当前elves运行的APP的版本信息为 other.localAppInfo 的数据。

当 other.switch设置为0，heartbeat会从supervisor模块获取。

