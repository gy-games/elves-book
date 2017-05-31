# 介绍

目前市面上的自动化运维工具众多，如Ansible,Saltstack,Puppet等均为非常优秀的自动化运维工具或平台，其主要面向运维人员使用命令行等方式进行自动化运维操作，Elves与上述自动化运维工具的初衷一致，但其致力于为运维开发人员提供服务， Elves为一套自动化运维开发平台，其自身不提供业务性功能，运维开发人员可根据自身的业务进行应用\(APP\)的开发来实现相应业务的自动化管理。

# 特性

灵活的业务\(App\)编程设计：Elves主要面向运维开发人员，以编程方式实现某业务的自动化操作，Elves与用户间交互以RESTful方式进行，与Apps间交互以进程调用方式进行，理论上支持所有的编程语言，目前Elves提供Python与C\#版开发SDK

任务模式：Elves提供[及时任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(同步\)，[队列任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(异步,支持依赖\)，[计划任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(异步\) 三种任务调度模式，且允许开发者直接将App-worker的执行结果直接反馈至App-processor，以构建C/S架构服务

高可用与高性能：在Elves的设计中各组件为可拔插形式，且极大程度的降低各组件间依赖关系，几乎所有组件均可以独立使用与集群部署

数据交互传输：Elves-Center间各组件的数据传输使用RABBITMQ以队列形式进行交互，Elves-Center与Elves-Agent间数据传输使用Thrift进行交互，开发人员操作Elves\(App\)使用RESTful方式交互

开发语言与结构：Elves自身以C/S架构设计，Elves-Center\(SERVER\)由JAVA实现，Elves-Agent\(CLIENT\)由Golang实现\(同时提供JAVA版本\)

# 架构![](/assets/arc.png)

Elves以三大核心组成，分别为"Elves-Center"，"Elves-Agent"，"Elves-Apps"

# Elves-Apps

Apps的便捷开发为整个系统的核心关注点，Elves-App分为两部分组成，分别为worker与processor，其中Worker运行在Agent端，简单点说如果想实现一个C/S架构的App则需要实现Worker与Processor，若只需要一个C架构的App则仅需要实现Worker。在Worker的指令调用中，我们采用进程调用方式，为进一步简化开发人员的工作，我们在Agent端引入了一个Proxy的概念，Proxy接收到指令后，将指令转换为方法以及参数并采用动态加载的方式调用App并将APP执行结构格式化后反馈至Elves。

以Python为例，开发人员只需要确定方法名、参数（Dictionary）实现自身逻辑即可。

**App "apptest" Worker 实现**

```py
  class apptest():
      def helloword(self,param):
          flag = "false"
          try:
              result = param["my"]
              flag = "true"
          except Exception,e:
        result = e
    return flag,result
```

如下用户只需要告知Elves，我要执行apptest的helloword方法，参数为{"my":"toryzen"}即可得到toryzen的反馈结构

APP的执行后需要返回两个执行结构，分别为flag与result，均为string类型，flag为枚举:true,false用于确定app的执行状态，result为执行反馈结果。

若想获取反馈结果可以通过Elves API或者实现一个Processor直接由Agent端主动反馈，Processor的实现也非常简单，可以在后续的使用手册中查看。

# Elves-Center

Elves-Center简单点说就是Elves的Server端，我们采用微服务为整个Elves提供高可用的服务。

## Scheduler

任务调度组件：负责发送APP任务至Elves-Agent

## Cron

计划任务组件：负责以Elves-Center为起点的计划任务调用组件

## Queue

队列任务组件：负责队列任务的调度执行

## Heartbeat

心跳组件：负责获取Agent的运行状态，且Elves-agent通过此组件获取其可执行的APP清单

## Supervisor

权限控制组件：负责ElvesAPP的管理与各Agent上APP的权限管理，此组件提供WEB化管理

![](/assets/elves-center-supervisor.png)

## OpenAPI

API：提供以RESTfulAPI方式操作Elves的各项接口

## CMDBProxy

CMDB数据提供代理：为Elves中各组件提供自身CMDB数据信息，便于Elves与自身运维体系的融合

## Watcher

Elves-Center的各组件监控：监听Elves-Center中各组件的运行状态，数据的传输等，便于运维Elves与进行Elves指令审计，Watcher的监听方式以RabbitMQ bind \*.\*的方式进行，对Elves-Center的业务处理无影响，并将监听结果存储至MangoDB供后续查询。

## Dashbord

为运维Elves的运维人员提供一套方便的WEB化管理界面

![](/assets/elves-center-dashbord.png)

# Elves-Agent

Elves-agent负责APP的下载与调用，执行结果的反馈与上报，以及当前Elves-Agent运行状态的监控，同时ElvesAgent也提供自身Cron功能、一个Web面板，且为了方便APP的开发，Elves-Agent提供开发者模式，提供简易版API直接供开发交互。

![](/assets/elves-agent-dashbord.png)

