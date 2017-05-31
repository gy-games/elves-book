# OpenAPI

OpenAPI组件为Elves-Center的唯一入口，对内采用RabbitMQ的方式与各组件进行交互，对外采用RESTful方式交互。OpenAPI中设置了大量的开关以达到组件的拔插性

OpenAPI为WEB项目，推荐部署至Tomcat容器下。

## 修改配置

**./src/main/resource/conf.properties**

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

#Auth Config                
auth.mode = supervisor              #权限模式，可选择supervisor或simple模式
auth.id   = 1                       #simple模式的auth_id
auth.key  = 123456                  #simple模式的auth_key

#Elves-Center Module Config
cron.enabled  = true                 #cron组件开关，关闭后，OpenAPI不再提供计划任务相关接口
queue.enabled = true                 #queue组件开关，关闭后，OpenAPI不再提供队列任务相关接口
```

**开启simple模式后无法使用supervisor的权限认证且simple模式提供的auth**_**id与auth**_**key可以管理并执行所有Elves的Agents下的所有Apps**

## 组件构建

```
cd ./openapi
./control build
cp ./openapi/ROOT.war {Tomcat目录}
{start Tomcat}
{访问:http://ip:port}
```

# API服务

[详见API](/api.md)

