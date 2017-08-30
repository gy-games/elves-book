# OpenAPI

OpenAPI组件为Elves-Center的唯一入口，对内采用RabbitMQ的方式与各组件进行交互，对外采用RESTful方式交互。OpenAPI中设置了大量的开关以达到组件的拔插性

OpenAPI为WEB项目，推荐部署至Tomcat容器下。

## 编译

```
cd elves-openapi
chmod +x ./control
./control build                                                 #二进制版本可以忽略编译过程
```

## 配置

```
mv conf/conf.properties.example conf/conf.properties            #复制配置文件
vim conf/conf.properties                                        #编辑配置文件
```


## 修改配置

**./conf/conf.properties**

```
#api server config
server.port=80                                                    #提供服务的端口

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


## 服务使用列表

| **组件** | **服务** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| scheduler | syncJob | rpc.call | 发送同步任务 |
| cron | createCron | rpc.call | 添加Cron计划任务 |
|  | startCron | rpc.call | 开启Cron计划任务 |
|  | stopCron | rpc.call | 停止Cron计划任务 |
|  | deleteCron | rpc.call | 删除Cron计划任务 |
|  | cronDetail | rpc.call | 计划任务详情 |
|  | cronList | rpc.call | 计划任务列表信息 |
| queue | createQueue | rpc.call | 创建队列 |
|  | addTask | rpc.call | 添加任务项 |
|  | commitQueue | rpc.call | 提交队列 |
|  | stopQueue | rpc.call | 停止队列 |
|  | queueResult | rpc.call | 获取队列执行结果 |
| supervisor | appAuthInfo | rpc.call | 获取实时agent在线数据 |
|  | getAuthKey | rpc.call | 通过authId获取authKey |
|  | validateAuth | rpc.call | 权限验证（authId是否有该IP运行app的权限） |
|  | appInfo | rpc.call | 获取authId管理的app数据 |

# 服务API

[详见API](/api.md)

