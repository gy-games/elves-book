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
略
```

## 安装MYSQL

```
略
```

## 安装ZOOKEEPER

```
略
```

## 安装MANGODB

```
略
```

## 安装TOMCAT

```
略
```

## 安装Apache或Nginx

```
略
```



