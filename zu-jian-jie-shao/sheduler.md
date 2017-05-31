# Scheduler

elves的任务调度组件。   接收cron组件、queue组件、openapi组件发起的任务指令，调度转发到agent并回收任务处理结果，最后将结果返回给任务发起方。

## 修改配置

**./elves-scheduler/conf/conf.properties**

```
#zookeeper config
zookeeper.host=192.168.6.117
zookeeper.outTime=10000
zookeeper.root = /elves

#mq config
mq.ip=192.168.6.117
mq.port=5672
mq.user=root
mq.password=root
```

## 组件构建





