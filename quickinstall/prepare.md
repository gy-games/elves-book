# 环境准备

因不同组件依赖的服务不同，可根据情况自行部署

| **组件** | 公共依赖**服务** | 特殊依赖服务 |
| :--- | :--- | :--- |
| Scheduler | RabbitMQ,Zookeeper | - |
| Queue | RabbitMQ,Zookeeper | Mysql |
| Cron | RabbitMQ,Zookeeper | Mysql |
| Heartbeat | RabbitMQ,Zookeeper | Apache或Nginx |
| Supervisor | RabbitMQ,Zookeeper | Mysql,Tomcat,FTP |
| OpenAPI | RabbitMQ,Zookeeper | - |
| Watcher&Dashbord | RabbitMQ,Zookeeper | MongoDB,Tomcat |
| Elves-Agent | - | - |

---

## 安装RABBITMQ

```bash
步骤略
详见：http://www.rabbitmq.com/download.html
```

## 安装MYSQL

```
略

Mysql用于Cron组件，Queue组件与Superviror组件，若不需要安装以上组件可以忽略
```

## 安装ZOOKEEPER

```
略

Zookeeper安装后需要创建elves根节点，zookeeper在整个ELVES中提供服务发现与选举的功能实现
```

## 安装TOMCAT

```
略

Tomcat主要用于openapi组件与supervior组件，当然也可以不使用Tomcat将以上WEB项目部署至其他容器中。
```

## 安装Apache或Nginx

```
略

apache或nginx主要提供WEB下载功能
```



