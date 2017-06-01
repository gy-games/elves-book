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

queue模块主要对openapi模块提供队列任务的操作接口，具体如下：

##### createQueue：

```
接收消息：
{
    "mqkey":"openapi.queue.createQueue",
    "mqtype":"call.EC0EF718FCC4130",
    "mqbody":{
        "app":"testApp"
    }
}

回复消息："发送RoutingKey:EC0EF718FCC4130"
{
    "mqkey":"queue.openapi",
    "mqtype":"cast",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e"
    }
}
```

##### createTask:

```
接收

{
    "mqkey":"openapi.queue.createTask",
    "mqtype":"call.EC0EF718FCC41307",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e",
        "ip":"192.168.1.1",
        "mode":"NP",
        "func":"test",
        "param":"",
        "timeout":20,
        "proxy":"",
        "depend_task_id":""
    }
}
回复消息："发送RoutingKey:EC0EF718FCC41307"
{
    "mqkey":"queue.openapi",
    "mqtype":"cast",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e"
    }
}
```

##### 停止队列

```
接收

{
    "mqkey":"openapi.queue.stopQueue.EC0EF718FCC41307",
    "mqtype":"cast",
    "json_queue_ids":["BF0EE718FCC41307","EC0EF718FCC41307"]
}
```

##### 获取队列任务信息接口

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

**./elves-queue/conf/conf.properties**

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



