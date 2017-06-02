# Supervisor

supervisor组件负责elves的APP管理与权限管理，提供web管理端提供这些数据 管理。

## 数据库

supervisor的权限管理和APP管理依赖mqsql数据库，下面是数据库SQL。

    CREATE TABLE `auth_key` (
      `auth_id` varchar(16) NOT NULL COMMENT '权限ID',
      `auth_key` varchar(16) NOT NULL COMMENT '权限key',
      `auth_name` varchar(20) NOT NULL COMMENT '名称',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `update_time` datetime NOT NULL COMMENT '权限修改时间',
      PRIMARY KEY (`auth_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='权限表'



    CREATE TABLE `app` (
      `app_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
      `instruct` varchar(30) NOT NULL COMMENT '指令',
      `app_name` varchar(30) NOT NULL COMMENT 'app名称',
      `founder` varchar(20) NOT NULL COMMENT '创建者',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `version_id` int(11) DEFAULT NULL COMMENT '当前版本id',
      `processor_ip` varchar(15) DEFAULT NULL COMMENT 'processor的ip',
      `processor_port` int(11) DEFAULT NULL COMMENT 'processor的port',
      PRIMARY KEY (`app_id`)
    ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='应用信息表'

## 组件服务

supervisor作为权限模块，主要对外提供：APP和权限相关的数据信息。

**RoutingKey : \*.supervisor**

### 服务提供列表

| **服务** | **类型** | **注解** |
| :--- | :--- | :--- |
| appAuthInfo | rpc.call | app信息和app绑定agent数据 |
| getAuthKey | rpc.call | 通过authId获取authKey |
| validateAuth | rpc.call | 权限验证（authId是否有该IP运行app的权限） |
| appInfo | rpc.call | 获取authId管理的app数据 |

### 服务提供详情

##### appAuthInfo：

```
接收消息：
{
    "mqkey":"{组件}.supervisor.appAuthInfo",
    "mqtype":"call.DDDEF718FCC41307",
    "mqbody":{
    }
}

回复消息："发送RoutingKey:DDDEF718FCC41307"
{
    "mqkey":"supervisor.{组件}",
    "mqtype":"cast",
    "mybody":{
        "result":[
            {
                "app":"appTest",
                "version":"1.0.0",
                "agentList":["192.168.1.1","192.168.1.2"]
            },
            {
                 "app":"appTest2",
                 "version":"1.0.1",
                 "agentList":["192.168.1.3","192.168.1.2"]
            }
        ]
    }
}
```

##### getAuthKey：

```
接收消息：
{
    "mqkey":"{组件}.supervisor.getAuthKey",
    "mqtype":"call.DDFEF718FCC41307",
    "mqbody":{
        "auth_id":"AAAAA718FCC41307"
    }
}

回复消息："发送RoutingKey:DDFEF718FCC41307"
{
    "mqkey":"supervisor.{组件}",
    "mqtype":"cast",
    "mybody":{
        "auth_key":"718FCC41307BBBBB"
    }
}
```

##### validateAuth：

```
接收消息：
{
    "mqkey":"{组件}.supervisor.validateAuth",
    "mqtype":"call.GGFEF718FCC41307",
    "mqbody":{
        "auth_id":"AAAAA718FCC41307",
        "app":"appTest",
        "ip":"192.168.1.1"
    }
}

回复消息："发送RoutingKey:GGFEF718FCC41307"
{
    "mqkey":"supervisor.{组件}",
    "mqtype":"cast",
    "mybody":{
        "result":"true"
    }
}
```

##### appInfo：

```
接收消息：
{
    "mqkey":"{组件}.supervisor.validateAuth",
    "mqtype":"call.OOFEF718FCC41307",
    "mqbody":{
        "auth_id":"AAAAA718FCC41307"
    }
}

回复消息："发送RoutingKey:OOFEF718FCC41307"
{
    "mqkey":"supervisor.{组件}",
    "mqtype":"cast",
    "mybody":{
        "result":{
            "app": "apptest",
            "app_name": "测试APP",
            "app_ver": "1.0.0"
        }
    }
}
```

##### agentList：

```
接收消息：
{
    "mqkey":"{组件}.supervisor.agentList",
    "mqtype":"call.OOFEF718FCC41307",
    "mqbody":{
        "auth_id":"AAAAA718FCC41307"
    }
}

回复消息："发送RoutingKey:OOFEF718FCC41307"
{
    "mqkey":"supervisor.{组件}",
    "mqtype":"cast",
    "mybody":{
        "result":[
            "127.0.0.1",
            "192.168.0.1",
            "172.32.0.1"
        ]
    }
}
```

## 修改配置

**./src/main/resource/conf.properties**

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

#FTP adress config
ftp.res.ip=http://192.168.0.1
ftp.res.user=admin
ftp.res.pass=admin
```

## 组件构建

```
cd ./elves-supervisor
mvn package
cp ./elves-supervisor/ROOT.war {Tomcat目录}
{start Tomcat}
{访问:http://ip:port}
```

## WEB功能说明

##### 登录

默认帐号：admin

默认密码：admin

![](/supervisor-img/login.png)

##### 密钥管理

##### App管理

##### Agent数据

##### 用户权限



