# Queue

queue组件是elves的队列任务组件，可以管理elves的队列任务，根据队列任务内容向scheduler发起任务，队列使用MqSQL实现。

## 修改配置

**./elves-queue/conf/conf.properties**

```
#zookeeper config
zookeeper.host=192.168.6.117
zookeeper.outTime=10000
zookeeper.node = /Elves/Queue/

#mq config
mq.ip=192.168.6.117
mq.port=5672
mq.user=root
mq.password=root

#jdbc conf
jdbc.type=mysql
jdbc.driver=com.mysql.jdbc.Driver
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20
jdbc.testSql=SELECT 'x' FROM DUAL
jdbc.url=jdbc\:mysql\://192.168.6.116\:3306/elves_queue?characterEncoding=UTF-8&amp;useOldAliasMetadataBehavior=true&amp;zeroDateTimeBehavior=convertToNull
jdbc.username=mysql
jdbc.password=mysql
```



