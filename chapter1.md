# 快速安装

Elves部署分为两部分，一部分为Elves-Center的部署，另一部分为Elves-Agent的部署。

**Elves-Center**的设计采用微服务架构，且已经最大程度将各组件间依赖关系降至最低，大家可根据自身需要进行组装部署。若只需要执行及时任务，center最小化部署为：scheduler，openapi，部署agent后，此时Elves将提供一个没有权限认证，无法进行APP自动更新的一个仅供运行及时任务的系统。若需要增加APP的自动下载，可以继续部署heartbeat组件；需要增加权限控制，可以继续部署supervisor组件；若需要增加队列任务，可以继续部署queue组件；若需要增加计划任务，可继续部署cron组件；若需要增加系统的整体监控，可继续部署Watcher与Dashbord组件。

后续我们也将提供一套DOCKER环境，提供Elves的快速搭建体验

**Elves-Agent**的安装非常简单，二进制包环境下，linux仅需要执行./control start，Windows需要先执行 control.cmd install ,在执行control.cmd start

