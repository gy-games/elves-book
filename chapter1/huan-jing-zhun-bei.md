# 环境准备

因不同组件依赖的服务不同，可根据情况自行部署

| **组件** | 公共依赖**服务** | 特殊依赖服务 |
| :--- | :--- | :--- |
| Scheduler | RabbitMQ,Zookeeper | - |
| Queue | RabbitMQ,Zookeeper | Mysql |
| Cron | RabbitMQ,Zookeeper | Mysql |
| Heartbeat | RabbitMQ,Zookeeper | - |
| Supervisor | RabbitMQ,Zookeeper | Mysql,Tomcat |
| OpenAPI | RabbitMQ,Zookeeper | - |
| Watcher | RabbitMQ,Zookeeper | MongoDB,Tomcat |
| Dashborad | RabbitMQ,Zookeeper | Tomcat |

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



