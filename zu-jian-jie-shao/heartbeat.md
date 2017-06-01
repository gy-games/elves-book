# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

## 组件服务

heartbeat模块主要对其他模块提供elves-agent的实时在线数据接口，具体如下：

##### agentInfo：

```
接收消息：
{
    "mqkey":"{模块}.heartbeat.agentInfo",
    "mqtype":"call.CCCEF718FCC41307",
    "mqbody":{
        "ip":"192.168.1.1",
        "asset":"CX0001"
    }
}

回复消息：
{
    "mqkey":"heartbeat.{模块}",
    "mqtype":"cast",
    "mqbody":{
        "result":[
            {
                "ip":"192.168.1.1",
                "asset":"CX0001",
                "last_hb_time":"2017-06-02 01:36:00",
                "apps":{
                    "testApp":"1.0.1"
                }
            }
        ]
    }
}
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
#Zookeeper地址
zookeeper.host=192.168.0.1
#Zookeeper超时时间
zookeeper.outTime=10000
#Zookeeper ROOT地址        
zookeeper.root=/elves  

#MQ Basic Config
#RabbitMQ IP
mq.ip       = 192.168.0.1
#RabbitMQ 端口
mq.port     = 5672
#RABBITMQ 账号
mq.user     = admin
#RABBITMQ 密码
mq.password = 1234567890
#Exchange 名称        
mq.exchange = elves


#auth config
#权限模式，可选择supervisor或simple模式
auth.mode = supervisor
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



