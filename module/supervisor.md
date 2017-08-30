# Supervisor

supervisor组件负责elves的APP管理与权限管理，提供web管理端提供这些数据 管理。

## 编译

```
cd elves-openapi
chmod +x ./control
./control build                                                 #二进制版本可以忽略编译过程
```

## 配置

```
mv conf/conf.properties.example conf/conf.properties            #复制配置文件
vim conf/conf.properties                                        #编辑配置文件
```

## 修改配置

**./conf/conf.properties**

```
#Zookeeper Config
zookeeper.host=127.0.0.1                 #Zookeeper地址
zookeeper.outTime=10000                  #Zookeeper超时时间      
zookeeper.root=/elves                    #Zookeeper超时时间

#MQ Basic Config
mq.ip       = 127.0.0.1                  #RabbitMQ IP
mq.port     = 5672                       #RabbitMQ 端口
mq.user     = admin                      #RABBITMQ 账号
mq.password =                            #RABBITMQ 密码      
mq.exchange = elves                      #RABBITMQ 密码

#FTP adress config
ftp.res.ip=http://127.0.0.1              #存储app安装包的FTP地址
ftp.res.user=admin                       #FTP帐号
ftp.res.pass=admin                       #FTP密码
```

## 脚本参数


**./control**

```
build|pack|start|stop|restart|status|version

build   : 运行后将执行mvn pakcge , 最终构建成至 bin
pack    : 将本模块打包(不包含配置文件与日志文件)
start   : 以nohup形式启动elves-{module}
stop    : 关闭elves-{module}
restart : 执行 stop & start
status  : 查看elves-{module}的运行状态
version : 查看当前模块的版本
```

## Mysql数据结构

supervisor的权限管理和APP管理依赖mqsql数据库，下面是数据库SQL。

    CREATE TABLE `app` (
      `app_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
      `instruct` varchar(30) NOT NULL COMMENT '指令',
      `app_name` varchar(30) NOT NULL COMMENT 'app名称',
      `founder` varchar(20) NOT NULL COMMENT '创建者',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `update_time` datetime DEFAULT NULL COMMENT '最后更新时间',
      `version_id` int(11) DEFAULT NULL COMMENT '当前版本id',
      PRIMARY KEY (`app_id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='应用信息表';

    CREATE TABLE `app_agent` (
      `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
      `app_id` int(11) NOT NULL COMMENT 'app主键ID',
      `ip` varchar(20) NOT NULL COMMENT 'ip',
      `asset_id` varchar(20) NOT NULL COMMENT '资产ID',
      PRIMARY KEY (`id`)
    ) ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='app绑定的agent列表';

    CREATE TABLE `app_version` (
      `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
      `version` varchar(30) NOT NULL COMMENT '版本号',
      `operator` varchar(20) NOT NULL COMMENT '创建人',
      `create_time` datetime NOT NULL COMMENT '版本创建时间',
      `app_id` int(11) NOT NULL COMMENT 'app主键ID',
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='app版本信息表';

    CREATE TABLE `auth_key` (
      `auth_id` varchar(16) NOT NULL COMMENT '权限ID',
      `auth_key` varchar(16) NOT NULL COMMENT '权限key',
      `auth_name` varchar(20) NOT NULL COMMENT '名称',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `app_id` int(11) NOT NULL COMMENT 'auth_id绑定的app_id',
      PRIMARY KEY (`auth_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='权限表';

    CREATE TABLE `user` (
      `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
      `email` varchar(40) NOT NULL COMMENT '邮箱',
      `password` varchar(40) NOT NULL COMMENT '密码',
      `user_name` varchar(20) NOT NULL COMMENT '用户名',
      `founder` varchar(20) NOT NULL COMMENT '创建人',
      `create_time` datetime NOT NULL COMMENT '创建时间',
      `update_time` datetime NOT NULL COMMENT '更新时间',
      `last_login_time` datetime DEFAULT NULL COMMENT '最后登录时间',
      `last_login_ip` varchar(15) DEFAULT NULL COMMENT '最后登录ip地址',
      `login_times` int(11) DEFAULT '0' COMMENT '登录次数',
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';

    insert  into `user`(`id`,`email`,`password`,`user_name`,`founder`,`create_time`,`update_time`,`last_login_time`,`last_login_ip`,`login_times`) values (1,'admin@gyyx.cn','21232f297a57a5a743894a0e4a801fc3','管理员','管理员','2017-06-19 18:12:28','2017-06-19 18:12:31','2017-06-20 16:21:21','127.0.0.1',0);

## 组件服务

supervisor作为权限模块，主要对外提供：APP和权限相关的数据信息。

**RoutingKey : \*.supervisor**

### 服务提供列表

| **服务** | **类型** | **注解** |
| :--- | :--- | :--- |
| [appAuthInfo](#appauthinfo) | rpc.call | app信息和app绑定agent数据 |
| [getAuthKey](#getauthkey) | rpc.call | 通过authId获取authKey |
| [validateAuth](#validateauth) | rpc.call | 权限验证（authId是否有该IP运行app的权限） |
| [appInfo](#appinfo) | rpc.call | 获取authId管理的app数据 |

### 服务提供详情

##### appAuthInfo： {#appauthinfo}

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

##### getAuthKey： {#getauthkey}

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
        "flag":"true",
        "error":"",
        "auth_key":"718FCC41307BBBBB"
    }
}
```

##### validateAuth： {#validateauth}

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
        "flag":"true",
        "error":"",
        "result":"true"
    }
}
```

##### appInfo： {#appinfo}

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
        "flag":"true",
        "error":"",
        "result":{
            "app": "apptest",
            "app_name": "测试APP",
            "app_ver": "1.0.0"
        }
    }
}
```

##### agentList： {#agentlist}

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
        "flag":"true",
        "error":"",
        "result":[
            "127.0.0.1",
            "192.168.0.1",
            "172.32.0.1"
        ]
    }
}
```

## WEB功能说明

##### 登录

默认数据库导入 帐号：admin@gyyx.cn    ， 默认密码：admin

![](/assets/supervisor-login.png)

##### 密钥管理![](/assets/supervisor-authkey.png)

##### App管理![](/assets/supervisor-app.png)

##### Agent数据![](/assets/supervisor-agentlist.png)

##### 用户权限![](/assets/supervisor-manager.png)



