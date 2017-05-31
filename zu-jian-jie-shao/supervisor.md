# Supervisor

supervisor组件负责elves的APP管理与权限管理， 认证模块目前主要向OpenAPI提供服务。

## 数据库

supervisor的权限管理和APP管理依赖mqsql数据库，下面是数据库SQL。





## 组件服务

supervisor作为权限模块，主要对外提供：APP和权限相关的数据信息。

##### APP版本数据：

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

**./src/main/resource/conf.properties**

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

#FTP adress config
ftp.res.ip=http://192.168.0.1
ftp.res.user=admin
ftp.res.pass=admin
```

## 组件构建

```
cd ./elves-supervisor
mvn package
cp ./elves-supervisor/ROOT.war {Tomcat目录}
{start Tomcat}
{访问:http://ip:port}
```



