# 环境准备

因不同组件依赖的服务不同，可根据情况自行部署

| **组件** | **服务** |
| :--- | :--- |
| Scheduler | RabbitMQ,Zookeeper |
| Queue | RabbitMQ,Zookeeper,Mysql |
| Cron | RabbitMQ,Zookeeper,Mysql |
| Heartbeat | RabbitMQ,Zookeeper |
| Supervisor | RabbitMQ,Zookeeper,Mysql,Tomcat |
| OpenAPI | RabbitMQ,Zookeeper |
| Watcher | RabbitMQ,Zookeeper,MangoDB,Tomcat |
| DashBorad | RabbitMQ,Zookeeper,Tomcat |

---

## 安装RABBITMQ

**1.安装前准备**

```bash
wget [http://dl.fedoraproject.org/pub/epel/6/x86\_64/epel-release-6-8.noarch.rpm](http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)

rpm -ivh epel-release-6-8.src.rpm

wget -P /etc/yum.repos.d/ http://repos.fedorapeople.org/repos/peter/erlang/epel-erlang.repo 

yum clean all 

yum -y install erlang
```

**2.安装rabbitmq **

```bash
rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc 

wget http://www.rabbitmq.com/releases/rabbitmq-server/v2.8.5/rabbitmq-server-2.8.5-1.noarch.rpm 

rpm -ivh rabbitmq-server-2.8.5-1.noarch.rpm
```

**3.启动rabbitmq并设置开机启动 **

```bash
chkconfig rabbitmq-server on 

/sbin/service rabbitmq-server start
```

**4.检查rabbitmq是否启动 **

```bash
ps aux |grep rabbitmq
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



