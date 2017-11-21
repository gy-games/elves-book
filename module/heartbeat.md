# HeartBeat

heartbeat组件的作用是接收elves-agent的心跳包，向其他组件提供elves-agent的实时在线状态。elves-agent定时发送心跳包给heartbeat模块，heartbeat回复elves-agent上可以运行的App版本信息。

# 安装过程

## 编译

```
cd elves-scheduler
chmod +x ./control
./control build                                                 #二进制版本可以忽略编译过程
```

## 配置

```
mv  conf/conf.properties.example conf/conf.properties            #复制配置文件
vim conf/conf.properties                                         #编辑配置文件
```

**./conf/conf.properties**

```
#zookeeper config
zookeeper.enabled=false                #是否开启ZK
zookeeper.host=127.0.0.1:2181          #Zookeeper 超时时间
zookeeper.outTime=10000                #Zookeeper ROOT地址
zookeeper.root = /elves                #Zookeeper Root地址

#MQ Basic Config
mq.ip       = 192.168.0.1              #RabbitMQ IP
mq.port     = 5672                     #RabbitMQ 端口
mq.user     = admin                    #RABBITMQ 账号
mq.password = 1234567890               #RABBITMQ 密码
mq.exchange = elves                    #Exchange 名称   

#auth config
auth.mode = supervisor                                 #权限模式，可选择supervisor或simple模式
auth.localAppInfo={'app1':'1.0.1','app2':'1.0.2'}      #simple模式的app列表

#thrift config
thrift.heartbeat.port=11102             #HeartBeat接收来自Agent消息的端口



```

注：

auth.mode 用于标识supervisor模块是否开启（supervisor：开启，simple：关闭）。

当auth.mode 设置为supervisor，heartbeat返回给elves-agent可以运行的APP的版本信息为 auth.localAppInfo 的数据。

当 auth.mode 设置为simple，heartbeat返回给elves-agent可以运行的APP的版本信息会从elves-supervisor模块获取。

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



