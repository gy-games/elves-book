## Cron

cron 组件 是 elves计划任务功能模块，基于Quartz框架实现的。 实现了elves 计划任务的添加和删除，根据计划任务内容定时向scheduler模块发起任务。

## Quart介绍

cron 组件,各种计划任务的调度基于Quart框架实现（官方网站：[http://www.quartz-scheduler.org]()）

Quartz是一套非常高效的轻量级框架，这里提供一下Quartz的cron规则语法，如果想对Quartz有深入的了解，请自己查询相关资料学习。

| 位置 | 时间域 | 允许值 | 特殊值 |
| :--- | :--- | :--- | :--- |
| 1 | 秒 | 0-59 | , - \* / |
| 2 | 分钟 | 0-59 | , - \* / |
| 3 | 小时 | 0-23 | , - \* / |
| 4 | 日期 | 1-31 | , - \* ? / L W C |
| 5 | 月份 | 1-12 | , - \* / |
| 6 | 星期 | 1-7 | , - \* ? / L C \# |
| 7 | 年份（可选） | 1970－2099 | , - \* / |

## 数据库

con模块计划任务的存储使用mqsql实现，下面是SQL语句：

    CREATE TABLE `task_cron` (
      `id` varchar(16) NOT NULL COMMENT '主键Id',
      `agent_ip` varchar(15) NOT NULL COMMENT '客户端IP',
      `mode` enum('sap','sanp') NOT NULL COMMENT '模式(sap,sanp)',
      `app` varchar(32) NOT NULL COMMENT '模块',
      `func` varchar(32) NOT NULL COMMENT '方法',
      `param` text COMMENT '参数',
      `timeout` int(11) DEFAULT '0' COMMENT '超时时间',
      `proxy` varchar(15) DEFAULT NULL COMMENT '代理器',
      `rule` varchar(30) NOT NULL COMMENT '规则(quartz表达式)',
      `flag` tinyint(1) NOT NULL DEFAULT '0' COMMENT '状态(0:暂停,1:正常)',
      `create_time` datetime NOT NULL COMMENT '任务接收时间',
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='计划任务表'

## 组件服务

cron模块主要对openapi模块提供计划任务的操作接口，具体如下：



## 修改配置

**./elves-cron/conf/conf.properties**

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


#jdbc conf
jdbc.type=mysql
jdbc.driver=com.mysql.jdbc.Driver
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20
jdbc.testSql=SELECT 'x' FROM DUAL
jdbc.url=jdbc\:mysql\://192.168.6.116\:3306/elves_cron?characterEncoding=UTF-8&amp;useOldAliasMetadataBehavior=true&amp;zeroDateTimeBehavior=convertToNull
jdbc.username=mysql
jdbc.password=mysql
```

## 组件构建

```
cd ./elves-cron/bin
./configure --prefix=/opt/elves/elves-cron
./make
./make install
```

```

```



