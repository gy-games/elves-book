# Scheduler

elves的任务调度组件。   接收cron组件、queue组件、openapi组件发起的任务指令，调度转发到agent并回收任务处理结果，最后将结果返回给任务发起方。

# 安装过程

## 编译

```
cd elves-scheduler
chmod +x ./control
./control build                                                 #二进制版本可以忽略编译过程
```

## 配置

```
mv conf/conf.properties.example conf/conf.properties            #复制配置文件
vim conf/conf.properties                                        #编辑配置文件
```

**./conf/conf.properties**

```
#Zookeeper Config
zookeeper.host=127.0.0.1:2181   #Zookeeper地址
zookeeper.outTime=10000                                          #Zookeeper超时时间
zookeeper.root=/elves                                            #Zookeeper ROOT地址  

#MQ Basic Config
mq.ip       = 126.0.0.1                                          #RabbitMQ IP
mq.port     = 5672                                               #RabbitMQ 端口
mq.user     = admin                                              #RABBITMQ 账号
mq.password =                                                    #RABBITMQ 密码
mq.exchange = elves                                              #Exchange 名称


#thrift server config
thrift.scheduler.port=10101                                      #schduler服务端口
thrift.agent.port=11101                                          #agent服务端口
thrift.outTime=30000                                             #thrif连接超时时间
```

## 脚本参数

** ./control**

```
build|pack|start|stop|restart|status|version

build   : 运行后将执行mvn pakcge , 最终构建成至 bin
pack    : 将本模块打包(不包含配置文件与日志文件)
start   : 以nohup形式启动elves-{module}
stop    : 关闭elves-{module}
restart : 执行 stop & start
status  : 查看elves-{module}的运行状态
version : 查看当前模块的版本
```

## Thrift 通讯服务

elves-scheduler 和elves-agent之间的通讯使用thrift实现，下面提供通讯thrift结构体和thrift服务：

```
//命令构体
struct Instruct{
    1 : string id,                      #消息ID
    2 : string ip,                      #AgentIP
    3 : string type,                    #类型cron,queue,rt
    4 : string mode,                    #模式P,NP
    5 : string app,                     #APP名称
    6 : string func,                    #APP 方法名
    7 : string param,                   #APP 方法参数
    8 : i32 timeout,                    #超时时间(秒)
    9 : string proxy                    #自定义入口
}

//命令结果结构体
struct Reinstruct{
    1 : Instruct ins,                    #命令结构体
    3 : i32 flag,                        #执行状态
    4 : i32 costtime,                    #执行耗时(毫秒)
    5 : string result                    #执行结果
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

scheduler与其它模块通讯使用rabbitmq实现，这里提供scheduler作为消费者，处理的消息数据结构。**scheduler组件监听RoutingKey : \*.schedler**

### 服务提供列表

| **服务** | **类型** | **注释** |
| :--- | :--- | :--- |
| [syncJob](#syncjob) | rpc.call | 发送同步任务 |
| [asyncJob](#asyncjob) | rpc.cast | 发送异步任务 |

### 服务提供详情

#### syncJob

```
接收消息：
{
    "mqkey":"{发送组件}.scheduler.syncJob",
    "mqtype":"call.88499CCA100F214",
    "mqbody":{
        "ip":"192.168.6.116",
        "app":"testApp",
        "func":"test',
        "param":"",
        "timeout":90,
        "proxy":""
    }
}

回复消息："发送RoutingKey:88499CCA100F214"
{
    "mqkey":"scheduler.{发送组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag"："true"
        "error":""
        "result":{
            "id":"9ad6af3b2e5d4c2f",
            "worker_flag":"1",
            "worker_message":"hello word!",
            "worker_costtime":"74"
        }
    }
}
```

#### asyncJob

```
接收消息：
{
    "mqkey":"{发送组件}.scheduler.asyncJob",
    "mqtype":"cast",
    "mqbody":{
        "id":"9ad6af3b2e5d4c2f"
        "ip":"192.168.6.116",
        "type":"cron",
        "mode":"NP",
        "app":"testApp",
        "func":"test',
        "param":"",
        "timeout":90,
        "proxy":""
    }
}
```

### 服务使用列表

| **组件** | **服务** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| queue | taskResult | cast | 发送队列任务处理结果 |
| cron | cronResult | cast | 发送计划任务处理结果 |



