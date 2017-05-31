# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

## 组件服务

heartbeat模块主要对其他模块提供elves-agent的实时在线数据接口，具体如下：

##### 获取agent实时在线数据：

```
接收

{
    "mqkey":"openapi.queue.infoQueue.EC0EF718FCC41307",
    "mqtype":"call",
    "json_queue_ids":["BF0EE718FCC41307","EC0EF718FCC41307"]
}

回复
｛
    "mqkey":"openapi.queue.infoQueue.EC0EF718FCC41307",
    "mqflag":1,
    "info":[
         {
            "id" : "BF0EE718FCC41307",
            "agent_ip" :"192.168.1.1",
            "mode" :"sap",
            "app" : "testapp",
            "func" :"mod1",
            "param" : "",
            "timeout" :5000,
            "proxy" : "test",
            "depend_tq_id" : "BF0EE718FCC41308",
            "flag" : "q"
        },
        ...
    ]

｝
```

##### 更新APP版本信息：

```
接收

{
    "mqkey":"openapi.queue.infoQueue.EC0EF718FCC41307",
    "mqtype":"call",
    "json_queue_ids":["BF0EE718FCC41307","EC0EF718FCC41307"]
}

回复
｛
    "mqkey":"openapi.queue.infoQueue.EC0EF718FCC41307",
    "mqflag":1,
    "info":[
         {
            "id" : "BF0EE718FCC41307",
            "agent_ip" :"192.168.1.1",
            "mode" :"sap",
            "app" : "testapp",
            "func" :"mod1",
            "param" : "",
            "timeout" :5000,
            "proxy" : "test",
            "depend_tq_id" : "BF0EE718FCC41308",
            "flag" : "q"
        },
        ...
    ]

｝
```



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

## 组件构建

```
cd ./elves-heartbeat/bin
./configure --prefix=/opt/elves/elves-heartbeat
./make
./make install
```



