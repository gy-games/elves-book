# Scheduler

elves的任务调度组件。   接收cron组件、queue组件、openapi组件发起的任务指令，调度转发到agent并回收任务处理结果，最后将结果返回给任务发起方。

## Thrift 服务

elves-scheduler 和elves-agent之间的通讯使用thrift实现。

```
//命令构体
struct Instruct{
    1 : string id,
    2 : string ip,
    3 : string type,
    4 : string mode,
    5 : string app,
    6 : string func,
    7 : string param,
    8 : i32 timeout,
    9 : string proxy
}

//命令结果结构体
struct Reinstruct{
    1 : Instruct ins,
    3 : i32 flag,
    4 : i32 costtime,
    5 : string result
}
```

```
//Scheduler提供接口
service SchedulerService{
    //异步返回结果处理器
    string dataTransport(1:Reinstruct reins)
}

//Agent提供接口
service AgentService{
    //指令接收器[异步]
    list<Reinstruct>  instructionInvokeAsync(1: list<Instruct> insList)
    //指令接收器[同步]
    Reinstruct instructionInvokeSync(1: Instruct ins)
}
```

## 组件服务

scheduler与其它模块通讯使用rabbitmq实现，这里提供scheduler作为消费者，处理的消息数据结构。

#### sendSync 发起同步任务

```
接收消息：
{
    "mqkey":"openapi.scheduler.sendSync.88499CCA100F214",
    "mqtype":"cast",
    "id":"88499CCA100F214"
    "agent_ip":"192.168.6.116",
    "mode":"sap",
    "app":"testApp",
    "func":"test',
    "param":"",
    "timeout":0,
    "proxy":""
}

回复消息：
{
    "mqkey":"scheduler.openapi.sendSync.88499CCA100F214",
    "mqtype":"cast",
    "mqflag":"1",
    "mqerror":"",
    "data":{
            "flag"：0
            "error":""
            "log_id":"BF0EE718FCC41307",
            "agent_ip":"192.168.1.1",
            "mode":"sap",
            "app":"testapp",
            "func":"mod1",
            "param":"",
            "timeout":5000,
            "proxy":"",
            "starttime":"2016-06-27 12:51:05"
            "agent_sync_flag":0,
            "agent_sync_error":"",
            "agent_sync_costtime":50,
            "worker_message":"",
            "worker_costtime":0,
            "processor_sync_flag":0,
            "processor_sync_error":"",
            "processor_sync_costtime":50
            "endtime":"2016-06-27 12:51:05",
            "result_flag":1
    }
}
```

#### sendAsync 发起异步任务

```
接收消息：
{
    "mqkey":"openapi.scheduler.sendAsync.88499CCA100F214",
    "mqtype":"call",
    "job_type":"cron/queue"
    "id":"88499CCA100F215",
    "rand_id":"88499CCA100F218"
    "agent_ip":"192.168.6.116",
    "mode":"sap",
    "app":"testApp",
    "func":"test',
    "param":"",
    "timeout":0,
    "proxy":""
}
```

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

```

```



