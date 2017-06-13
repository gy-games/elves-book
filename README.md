# 介绍

Elves为一套自动化运维开发平台\(IT Automatic Develop Platform\)，面向开发，注重以编程实现运维自动化，致力于为运维研发人员提供便捷的运维自动化业务编程实现环境， Elves自身不提供业务性功能，运维开发人员可根据自身的业务进行应用\(APP\)的开发来实现相应业务的自动化管理。

# 特性

灵活的业务\(App\)编程设计：Elves主要面向运维开发人员，以编程方式实现某业务的自动化操作，Elves与用户间交互以RESTful方式进行，与Apps间交互以进程调用方式进行，理论上支持所有的编程语言，目前Elves提供Python与C\#版开发SDK

任务模式：Elves提供[及时任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(同步\)，[队列任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(异步,支持依赖\)，[计划任务](/shi-yong-shou-ce/ren-wu-mo-shi.md)\(异步\) 三种任务调度模式，且允许开发者直接将App-worker的执行结果直接反馈至App-processor，以构建C/S架构服务

高可用与高性能：在Elves的设计中各组件为可拔插形式，且极大程度的降低各组件间依赖关系，几乎所有组件均可以独立使用与集群部署

数据交互传输：Elves-Center间各组件的数据传输使用RABBITMQ以队列形式进行交互，Elves-Center与Elves-Agent间数据传输使用Thrift进行交互，开发人员操作Elves\(App\)使用RESTful方式交互

开发语言与结构：Elves自身以C/S架构设计，Elves-Center\(SERVER\)由JAVA实现，Elves-Agent\(CLIENT\)由Golang实现

# 架构![](/assets/arc.jpg)

Elves逻辑上以三大核心组成，分别为"**Elves-Center**"，"**Elves-Agent**"，"**Elves-Apps**"

# Elves-Apps

Apps的便捷开发为整个系统的核心关注点，Elves-App分为两部分组成，分别为worker与processor，其中Worker运行在Agent端，简单点说如果想实现一个C/S架构的App则需要实现Worker与Processor，若只需要一个C架构的App则仅需要实现Worker。在Worker的指令调用中，我们采用进程调用方式，为进一步简化开发人员的工作，我们在SDK内置了Agent入口，入口接收到指令后，将指令转换为方法以及参数并采用动态加载的方式调用App并将APP执行结构格式化后反馈至Elves。

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

若想获取反馈结果可以通过Elves API或者实现一个Processor直接由Agent端主动反馈，Processor的实现也非常简单，可以在后续的使用手册中查看。[4.1调试HelloWork示例](/shi-yong-shou-ce/helloword.md)

# Elves-Center

Elves-Center简单点说就是Elves的Server端，我们采用微服务为整个Elves提供高可用的服务，所有组件均注册至Zookeeper检测存活与部分组件集群化部署选举，组件间的数据交互使用RABBITMQ以队列方式进行交互，各组件功能单一且耦合度极低。

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

API：提供HTTP RESTfulAPI方式操作Elves的各项接口

## \*CMDBProxy

CMDB数据提供代理：为Elves中各组件提供自身CMDB数据信息，便于Elves与自身运维体系的融合

## Watcher & Dashbord

Watcher实现对Elves-Center的各组件监控：监听Elves-Center中各组件的运行状态，数据的传输等，便于运维Elves与进行Elves指令审计，Watcher的监听方式以RabbitMQ bind \*.\*的方式进行，对Elves-Center的业务处理无影响，并将监听结果存储至MangoDB供后续查询。Dashbord为运维Elves的运维人员提供一套方便的WEB化管理界面

![](/assets/elves-center-dashbord.png)

# Elves-Agent

Elves-agent负责APP的下载与调用，执行结果的反馈与上报，以及当前Elves-Agent运行状态的监控，同时ElvesAgent也提供自身Cron功能、一个Web面板，且为了方便APP的开发，Elves-Agent提供开发者模式，提供简易版API直接供开发交互。

![](/assets/elves-agent-dashbord.png)

# 任务模式

在了解任务模式及类型前，我们先来了解一下Elves有哪些实现

* 以发起方式角度来看：一次APP的执行有两种方式：1、由Elves-Agent端发起，2、由Elves-Center发起；
* 从任务的执行方式开看有：1、异步执行，2、同步执行；
* 从APP执行的后续处理来看有：1、处理型，2、非处理型，处理型即APP执行结果将反馈至用户自定义的Processor，非处理型即执行结果不反馈至用户Processor

从上面三个角度出发共组装成了 5种 工作模式，并由归属于不同的任务

1. **AANP  Agent端发起，异步执行，非处理型**
2. **AAP     Agent端发起，异步执行，处理型**
3. **CANP  Center端发起，异步执行，非处理型**
4. **CAP     Center端发起，异步执行，处理型**
5. **CSNP   Center端发起，同步执行，非处理型**

![](/assets/modeandtype.png)

# 任务类型

Elves中共有三种类型的任务，分别为及时任务，队列任务与计划任务，从字面就可以理解其含义。

## 及时任务

及时任务只支持CSNP模式，用户通过API发送即时任务后将阻塞等待APP执行结果的反馈

## 队列任务

队列任务支持2种模式，1、CANP模式，2、CAP模式 用户需要使用API先创建队列，添加队列中任务，提交队列后任务开始执行，如需获取执行结果，使用方需要另行发起请求，Elves的队列任务支持任务间的依赖。

## 计划任务

计划任务支持4种模式，1、AANP模式，2、AAP模式，3、CANP模式，4、CAP模式。其中由Agent端发起的任务Elves-Center无法进行监控与追踪。用户可以根据规则设置有Elves-Center端发起的计划任务

