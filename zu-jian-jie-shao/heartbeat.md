# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。

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

注：other.switch 用于标识supervisor模块是否开启（0：开启，1：关闭），如果other.switch设置为1，elves-agent每次发送心跳包给heartbeat，heartbeat返回当前elves运行的APP的版本信息为 other.localAppInfo 的数据，如果other.switch设置为0，则heartbeat会从supervisor模块获取。

