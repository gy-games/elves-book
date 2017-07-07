# 快速安装

Elves部署分为两部分，一部分为**elves-center**的部署，另一部分为**elves-agent**的部署。

---

## ELVES-CENTER

**elves-center**的设计采用微服务架构，且已经最大程度将各组件间依赖关系降至最低，大家可根据自身需要进行各组件的组装部署。

**ELVES最小化部署**![](/assets/elves-mini-1.png)最小化部署仅支持 [及时任务](/api/rt/exec.md) 模式，且无权限认证，无APP自动更新部署功能

---

**增加APP自动下载功能**![](/assets/elves-mini-2.png)在最小化部署基础上，部署HeartBeat模块，并在Agent配置文件中开启HeartBeat,即可使用APP自动下载更新功能，APP列表可静态配置在HeartBeat的配置文件中。

---

**需要增加权限控制**，可以继续部署supervisor组件；

---

**若需要增加队列任务**，可以继续部署queue组件；

---

**若需要增加计划任务**，可继续部署cron组件；

---

**若需要增加系统的整体监控**，可继续部署Watcher与Dashbord组件。

---

后续我们也将提供一套DOCKER环境，提供Elves的快速搭建体验



---

## ELVES-AGENT

**elves-agent**的安装非常简单，二进制包环境下，linux仅需要执行./control start，Windows需要先执行 control.cmd install ,在执行control.cmd start

详见 [2.3安装Elves-Agent](/quickinstall/install-elves-agent.md)

