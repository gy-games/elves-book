# 快速安装

Elves部署分为两部分，一部分为**elves-center**的部署，另一部分为**elves-agent**的部署。

---

## ELVES-CENTER

**elves-center**的设计采用微服务架构，且已经最大程度将各组件间依赖关系降至最低，大家可根据自身需要进行各组件的组装部署。

**ELVES最小化部署**
![](/assets/elves-mini-1.png)

最小化部署仅支持 [及时任务](/api/rt/exec.md) 模式，且无权限认证，无APP自动更新部署功能

---

**增加APP自动下载功能**
![](/assets/elves-mini-2.png)

在最小化部署基础上，部署[HeartBeat](/module/heartbeat.md)模块，并在Agent配置文件中开启HeartBeat,即可使用APP自动下载更新功能，APP列表可静态配置在HeartBeat的配置文件中。

---

**需要增加权限控制，增加APP界面化管理**
![](/assets/elves-mini-3.png)

在上一版基础上增加[Supervisor](/module/supervisor.md)模块，可以增加Elves的权限管理功能，WEB UI方式管理密钥，APP。

---

**若需要增加异步队列任务（支持依赖关系的任务队列）**
![](/assets/elves-mini-4.png)

如需要增加队列任务，可以增加[Queue](/module/queue.md)模块，[队列任务](/api/queue.md)为异步任务，且支持依赖关系与结果反馈。

---

**若需要增加计划任务（集中化任务管理）**
![](/assets/elves-mini-5.png)

如果需要集中化的计划任务管理，可以增加[Cron](/module/cron.md)模块，[计划任务](/api/cron.md)支持到秒级。

---

**若需要增加系统的整体监控**，可继续部署[Watcher&Dashbord](/module/watcher.md)组件。
![](/assets/arc.jpg)

---

后续我们也将提供一套DOCKER环境，提供Elves的快速搭建体验

---

## ELVES-AGENT

**elves-agent**的安装非常简单，二进制包环境下，linux仅需要执行./control start，Windows需要先执行 control.cmd install ,在执行control.cmd start

详见 [2.3安装Elves-Agent](/quickinstall/install-elves-agent.md)

