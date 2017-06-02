# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

## 组件服务

heartbeat模块主要对其他模块提供elves-agent的实时在线数据接口，具体如下：

**RoutingKey : \*.heartbeat**

### 服务提供列表

| **服务** | **类型** | **注释** |
| :--- | :--- | :--- |
| agentInfo | rpc.call | 获取实时agent在线数据 |
| agentList | rpc.call | 搜索agent列表 |
| updateAppInfo | rpc.cast | 通知heartbeat更新app信息 |

### 服务提供详情

##### agentInfo：

```
接收消息：
{
    "mqkey":"{组件}.heartbeat.agentInfo",
    "mqtype":"call.CCCEF718FCC41307",
    "mqbody":{
        "ip":"192.168.1.1"
    }
}

回复消息："发送RoutingKey:CCCEF718FCC41307"
{
    "mqkey":"heartbeat.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "result":{
            "ip":"192.168.1.1",
            "asset":"CX0001",
            "last_hb_time":"2017-06-02 01:36:00",
            "apps":{
                "testApp":"1.0.1"
            }
        }
    }
}
```

##### agentList：

```
接收消息：
{
    "mqkey":"{组件}.heartbeat.agentList",
    "mqtype":"call.CCCEF718FCC41307",
    "mqbody":{
        "ip":"192.168.1.1",
        "asset":"",
        "pagesize":10,
        "pagenumber":1
    }
}

回复消息："发送RoutingKey:CCCEF718FCC41307"
{
    "mqkey":"heartbeat.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "count":1000,
        "result":[{
            "ip":"192.168.1.1",
            "asset":"CX0001",
            "last_hb_time":"2017-06-02 01:36:00",
            "apps":{
                "testApp":"1.0.1"
            }]
        }
    }
}
```

##### updateAppInfo：

```
接收消息：
{
    "mqkey":"{组件}.heartbeat.updateAppInfo",
    "mqtype":"cast",
    "mqbody":{
        "result":[{
            "app":"appTest",
            "version":"1.0.0",
            "agentList":["192.168.1.1","192.168.1.2"]
        }]
    }
}
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
mvn package
./control start
```



