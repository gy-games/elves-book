# Queue

queue组件是elves的队列任务组件，可以管理elves的队列任务，根据队列任务内容向scheduler发起任务，队列使用MqSQL实现。

## 数据库

queue模块计划任务的存储使用mqsql实现，下面是SQL语句。

##### task\_queue队列任务表：

    CREATE TABLE `task_queue` (
      `id` varchar(16) NOT NULL COMMENT '主键ID',
      `agent_ip` varchar(15) NOT NULL COMMENT 'AgentIP',
      `mode` enum('sap','sanp') NOT NULL COMMENT '模式',
      `app` varchar(32) NOT NULL COMMENT '模块',
      `func` varchar(32) NOT NULL COMMENT '指令',
      `param` longtext COMMENT '参数',
      `timeout` int(11) DEFAULT '0' COMMENT '超时时间',
      `proxy` varchar(15) DEFAULT NULL COMMENT '代理器',
      `depend_tq_id` varchar(16) DEFAULT NULL COMMENT '依赖id',
      `flag` tinyint(4) NOT NULL DEFAULT '0' COMMENT '状态(0:等待,1:运行,2:结束)',
      `call_id` varchar(16) DEFAULT NULL COMMENT '锁ID(自动生成):',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='队列任务表'

##### lock\_queue存储过程：

    DELIMITER $$

    CREATE DEFINER=`mysql`@`%` PROCEDURE `lock_queue`(IN T VARCHAR(16))
        BEGIN
                CREATE TEMPORARY TABLE locktmp (
                         `id` VARCHAR(16) DEFAULT NULL COMMENT '队列ID'
                        ) ENGINE=MEMORY DEFAULT CHARSET=utf8;
                INSERT INTO locktmp

                SELECT id FROM task_queue WHERE flag = 1  AND call_id IS NULL AND depend_tq_id IS NULL 
                UNION ALL 
                (SELECT c.id FROM(
                SELECT a.id,b.call_id,b.flag FROM 
                (SELECT * FROM task_queue WHERE flag = 1 AND call_id IS NULL AND  depend_tq_id IS NOT NULL) AS a
                LEFT JOIN task_queue b ON a.depend_tq_id =b.id
                WHERE b.call_id IS NOT NULL AND b.flag = 2) c
                );

                UPDATE `task_queue` SET call_id =T WHERE `id` IN( SELECT * FROM locktmp );
                DROP TEMPORARY TABLE locktmp;
        END$$

    DELIMITER ;

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



