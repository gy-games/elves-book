# Queue

queue组件是elves的队列任务组件，可以管理elves的队列任务，根据队列任务内容向scheduler发起任务，队列使用MqSQL实现。

## 数据库

queue模块计划任务的存储使用mqsql实现，下面是SQL语句：

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

**./elves-queue/conf/conf.properties**

```
#zookeeper config
zookeeper.host=192.168.6.117
zookeeper.outTime=10000
zookeeper.node = /Elves/Queue/

#mq config
mq.ip=192.168.6.117
mq.port=5672
mq.user=root
mq.password=root

#jdbc conf
jdbc.type=mysql
jdbc.driver=com.mysql.jdbc.Driver
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20
jdbc.testSql=SELECT 'x' FROM DUAL
jdbc.url=jdbc\:mysql\://192.168.6.116\:3306/elves_queue?characterEncoding=UTF-8&amp;useOldAliasMetadataBehavior=true&amp;zeroDateTimeBehavior=convertToNull
jdbc.username=mysql
jdbc.password=mysql
```

## 组件构建

```
cd ./elves-queue/bin
./configure --prefix=/opt/elves/elves-queue
./make
./make install
```

```

```



