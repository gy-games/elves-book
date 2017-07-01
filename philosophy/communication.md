# 组件间通讯

在Elves-Center中，各组件的通讯依托于RABBITMQ实现以此达到功能型组件可以实现集群化部署，降低各组件之间的耦合度。Elves中RabbitMQ的使用与OpenStack类似，实现OpenStack中rpc.call与rpc.cast两种类型

## 消息类型

**RPC-CALL**![](/assets/rpc-call.png)

**RPC-CAST**

![](/assets/rpc-cast.png)

## 内容规则

消息内容均以JSON化显示，保留

* mqkey    : 发送组件.接收组件.方法
* mqtype  : 发送类型\(call/cast\),若为call类型，需要加上其ID例如 call.SYDHAGSY6SYAHSGA
* mqbody : 消息参数

**rpc-call实例：**

RoutingKey : cron.sheduler \#向Agent发送异步非处理型指令，routingkey : cron.sheduler.callagent

```
{
    "mqkey"  : "cron.sheduler.callagent",
    "mqtype" : "cast"
    "mqbody" : {
        "app"  : "webops",
        "func" : "createSiteFun",
        "mode" : "p",
        "param": "{}"
    }
}
```

## 交互场景示例

#### 基础操作

* 权限认证\(openapi-&gt;supervisor-&gt;openapi\)： 简称oao，用于所有以OpenApi为基础的操作，第一步进行权限的认证
* 在线监测\(sheduler-&gt;heartbeat-&gt;sheduler\)：简称shs，用于所有Sheduler为基础的操作，作为第一步在线检测

#### 业务

* 计划任务管理操作\(oao-&gt;cron-&gt;openapi\)： 用于添加，删除计划任务操作
* 发起计划任务\(cron-&gt;heartbeat-&gt;cron-&gt;sheduler\)：用于定时任务的发起
* 发起异步任务\(sheduler-&gt;heartbeat -&gt; \*agent\)，由Sheduler发起计划任务
* 发起同步任务\(oao-&gt;sheduler-&gt;heartbeat-&gt;\*agent-&gt;sheduler-&gt;openapi\),由OpenApi发起同步任务



