# Scheduler

elves的任务调度组件。   接收cron组件、queue组件、openapi组件发起的任务指令，调度转发到agent并回收任务处理结果，最后将结果返回给任务发起方。

## Thrift 服务

elves-scheduler 和elves-agent之间的通讯使用thrift实现，下面提供通讯thrift结构体和thrift服务：

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

##### 同步任务

```
接收消息：
{
    "mqkey":"openapi.scheduler.syncJob",
    "mqtype":"call.88499CCA100F214",
    "mqbody":{
        "rt_id":"9ad6af3b2e5d4c2f"
        "ip":"192.168.6.116",
        "app":"testApp",
        "func":"test',
        "param":"",
        "timeout":90,
        "proxy":""
    }
}

回复消息：
{
    "mqkey":"scheduler.openapi.syncJob",
    "mqtype":"cast",
    "mqbody":{
        "flag"："true"
        "error":""
        "result":{
            "rt_id":"9ad6af3b2e5d4c2f",
            "worker_flag":"1",
            "worker_message":"hello word!",
            "worker_costtime":"74"
        }
    }
}
```

##### 异步queue任务

```
接收queue消息：
{
    "mqkey":"queue.scheduler.asyncJob",
    "mqtype":"cast",
    "mqbody":{
        "task_id":"9ad6af3b2e5d4c2f"
        "ip":"192.168.6.116",
        "app":"testApp",
        "func":"test',
        "param":"",
        "timeout":90,
        "proxy":""
    }
}

异步queue任务返回:
{
    "mqkey":"queue.scheduler.asyncJob",
    "mqtype":"cast",
    "mqbody":{
        "flag"："true"
        "error":""
        "result":{
            "task_id":"9ad6af3b2e5d4c2f",
            "worker_flag":"1",
            "worker_message":"hello word!",
            "worker_costtime":"74"
        }
    }
}
```

##### 异步cron任务

```
接收cron消息：
{
    "mqkey":"cron.scheduler.asyncJob",
    "mqtype":"cast",
    "mqbody":{
        "cron_id":"9ad6af3b2e5d4c2d"
        "ip":"192.168.6.116",
        "app":"testApp",
        "func":"test',
        "param":"",
        "timeout":90,
        "proxy":""
    }
}

异步cron任务返回消息:
{
    "mqkey":"scheduler.cron.asyncJob",
    "mqtype":"cast",
    "mqbody":{
        "flag"："true"
        "error":""
        "result":{
            "cron_id":"9ad6af3b2e5d4c2d",
            "worker_flag":"1",
            "worker_message":"hello word!",
            "worker_costtime":"74"
        }
    }
}
```

## 修改配置

**./elves-scheduler/conf/conf.properties**

```
#Zookeeper Config
#Zookeeper地址
zookeeper.host=10.0.101.1:2181,10.0.101.2:2181,10.0.101.3:2181
#Zookeeper超时时间
zookeeper.outTime=10000
#Zookeeper ROOT地址        
zookeeper.root=/elves  

#MQ Basic Config
#RabbitMQ IP
mq.ip       = 10.0.101.100
#RabbitMQ 端口
mq.port     = 5672
#RABBITMQ 账号
mq.user     = admin
#RABBITMQ 密码
mq.password = 1234567890
#Exchange 名称        
mq.exchange = elves
```

## 组件构建

```
cd ./elves-scheduler/bin
./configure --prefix=/opt/elves/elves-scheduler
./make
./make install
```



