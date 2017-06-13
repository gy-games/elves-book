# Queue

queue组件是elves的队列任务组件，可以管理elves的队列任务，根据队列任务内容向scheduler发起任务，队列使用MqSQL实现。

![](/assets/queue-flow.png)

## 编译及安装

```
wget {elves-queue.tar.gz}                     #选择合适的安装包
tar -zxvf {elves-queue.tar.gz}                #解压安装包
cd elves-queue                                #进入Elves-QUEUE目录
vim conf/conf.properties                      #修改配置文件
{./control build}                                #若为源码包可以进行构建，构建需要mvn环境                              
./control start                               #启动elves-queue
./control status                              #查看启动状态
```

## 配置文件

**./elves-queue/conf/conf.properties**

```
#Zookeeper Config
zookeeper.host=192.168.0.1      #Zookeeper地址
zookeeper.outTime=10000         #Zookeeper超时时间
zookeeper.root=/elves           #Zookeeper ROOT地址 

#MQ Basic Config
mq.ip       = 192.168.0.1       #RabbitMQ IP
mq.port     = 5672              #RabbitMQ 端口
mq.user     = admin             #RABBITMQ 账号
mq.password = 1234567890        #RABBITMQ 密码
mq.exchange = elves             #Exchange 名称  

#jdbc conf
jdbc.type=mysql
jdbc.driver=com.mysql.jdbc.Driver
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20
jdbc.testSql=SELECT 'x' FROM DUAL
jdbc.url=jdbc\:mysql\://192.168.0.1\:3306/elves_queue?characterEncoding=UTF-8&amp;useOldAliasMetadataBehavior=true&amp;zeroDateTimeBehavior=convertToNull
jdbc.username=mysql
jdbc.password=mysql
```

## Mysql数据库结构

queue模块计划任务的存储使用mysql实现，下面是数据库结构。

##### queue表：

    CREATE TABLE `queue` (
      `queue_id` varchar(16) NOT NULL COMMENT '队列ID',
      `app` varchar(25) DEFAULT NULL COMMENT 'APP',
      `createtime` datetime DEFAULT NULL COMMENT '创建时间',
      `committime` datetime DEFAULT NULL COMMENT '提交时间',
      `status` enum('pendding','running','stoped') DEFAULT 'pendding' COMMENT '队列状态',
      PRIMARY KEY (`queue_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8

##### task\_list表

    CREATE TABLE `task_list` (
      `task_id` varchar(16) NOT NULL COMMENT '任务ID',
      `queue_id` varchar(16) NOT NULL COMMENT '队列ID',
      `ip` varchar(15) NOT NULL COMMENT 'AgentIP',
      `mode` enum('np','n') NOT NULL COMMENT '模式',
      `app` varchar(32) NOT NULL COMMENT '模块',
      `func` varchar(32) NOT NULL COMMENT '指令',
      `param` text COMMENT '参数',
      `timeout` int(11) DEFAULT '0' COMMENT '超时时间',
      `proxy` varchar(35) DEFAULT NULL COMMENT '代理器',
      `depend_task_id` varchar(16) NOT NULL DEFAULT 'null' COMMENT '依赖的任务id',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `flag` int(11) DEFAULT NULL COMMENT '返回值',
      `error` text COMMENT '返回错误',
      `worker_flag` int(11) DEFAULT NULL COMMENT 'worker执行状态',
      `worker_message` text COMMENT 'worker执行结果内容',
      `worker_costtime` int(11) DEFAULT NULL COMMENT 'worker执行耗时',
      `exec_finish_time` datetime DEFAULT NULL COMMENT '结果回收时间',
      `status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '状态(0:等待,1:运行,2:结束)',
      PRIMARY KEY (`task_id`),
      KEY `queue_id` (`queue_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='队列任务表'

## 组件服务

queue模块主要对openapi模块提供队列任务的操作接口，具体如下：

**RoutingKey : \*.queue**

### 服务提供列表

| **服务** | **类型** | **注释** |
| :--- | :--- | :--- |
| [createQueue](#createqueue) | rpc.call | 创建队列 |
| [addTask](#addtask) | rpc.call | 添加任务项 |
| [commitQueue](#commitqueue) | rpc.call | 提交队列 |
| [stopQueue](#stopqueue) | rpc.call | 停止队列 |
| [queueResult](#queueresult) | rpc.call | 获取队列执行结果 |
| [taskResult](/taskResult) | rpc.cast | 队列任务直接结果处理 |

### 服务提供详情

##### createQueue： {#createqueue}

```
接收消息：
{
    "mqkey":"{组件}.queue.createQueue",
    "mqtype":"call.EC0EF718FCC4130",
    "mqbody":{
        "app":"testApp"
    }
}

回复消息："发送RoutingKey:EC0EF718FCC4130"
{
    "mqkey":"queue.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag":"true",
        "error":"",
        "id":"12d6af3b2e5d4c2e"
    }
}
```

##### addTask: {#addtask}

```
接收消息：
{
    "mqkey":"{组件}.queue.addTask",
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
    "mqkey":"queue.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag":"true",
        "error":"",
        "id":"ddd6af3b2e5d4c2e"
    }
}
```

##### commitQueue: {#commitqueue}

```
接收消息：
{
    "mqkey":"{组件}.queue.commitQueue",
    "mqtype":"call.EC0EF718FCC41388",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e"
    }
}

回复消息："发送RoutingKey:EC0EF718FCC41388"
{
    "mqkey":"queue.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag": "true",
        "error": ""
    }
}
```

##### stopQueue: {#stopqueue}

```
接收消息：
{
    "mqkey":"{组件}.queue.stopQueue",
    "mqtype":"call.AAAEF718FCC41307",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e"
    }
}

回复消息："发送RoutingKey:AAAEF718FCC41307"
{
    "mqkey":"queue.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag": "true",
        "error": "",
        "result": {
            "BF0EE718FCC41307": "finish",
            "AF0EE718FCC4130C": "execing",
            "EF0EE718FSEC4130": "stoped"
        }
    }
}
```

##### queueResult {#queueresult}

```
接收消息：
{
    "mqkey":"{组件}.queue.queueResult",
    "mqtype":"call.EC0EF718FCC41BBB",
    "mqbody":{
        "id":"12d6af3b2e5d4c2e"
    }
}

回复消息："发送RoutingKey:EC0EF718FCC41BBB"
{
    "mqkey":"queue.{组件}",
    "mqtype":"cast",
    "mqbody":{
        "flag": "true",
        "error": "",
        "result":{
            "BF0EE718FCC41307": {
                "status":"finish",
                "depend_task_id":"",
                "flag": "true",
                "error": "",
                "id": "BF0EE718FCC41307",
                "worker_flag": "1",
                "worker_message": "hello word!",
                "worker_costtime": "74"
            },
            "AF0EE718FCC41301": {
                "status":"execing",
                "depend_task_id":"BF0EE718FCC41307",
                "flag": "",
                "error": "",
                "id": "AF0EE718FCC41301",
                "worker_flag": "",
                "worker_message": "",
                "worker_costtime": ""
            },
            "CF0EE718FCC41302": {
                "status":"pendding",
                "depend_task_id":"BF0EE718FCC41307",
                "flag": "",
                "error": "",
                "id": "CF0EE718FCC41302",
                "worker_flag": "",
                "worker_message": "",
                "worker_costtime": ""
            }
        }
    }
}
```

##### taskResult： {#taskresult}

```
 {
    "mqkey":"{组件}.queue.taskResult",
    "mqtype":"cast",
    "mqbody":{
        "flag"："true"
        "error":""
        "result":{
            "id":"9ad6af3b2e5d4c2d",
            "worker_flag":"1",
            "worker_message":"hello word!",
            "worker_costtime":"74"
        }
    }
}
```

### 服务使用列表

| **组件** | **服务** | **类型** | **注释** |
| :--- | :--- | :--- | :--- |
| scheduler | asyncJob | cast | 发起异步任务 |



