# 基于ELVES实现的WEB自动化操作

光宇游戏的WEB自动化运维产品，这款产品光宇游戏内部的代号是TRON，提供WEB运维的CMDB管理，自动化操作，配置检测等相关的服务，其中自动化操作、与配置检测产品均基于ELVES实现。

实现的功能有：

* IIS操作,新建/修改站点、重启IIS
* Nginx操作,新增/修改Upstream,新增/修改servername
* SQUID操作,查看修改SQUIDHOSTS,PUREGE
* WEB节点HOSTS操作，查看/修改HOSTS
* 配置文件查看，操作
* 数据库主从切换，SQL SERVER AWO 切换
* GIT版本检测
* 等等

使用的模式有：

* 及时任务模式 rt ，CSNP
* 队列任务模式 queue ，CANP

## 新站点一键部署：

目前可以实现IIS站点的一键部署与Tomcat站点的一键部署，直接在目标机器上建立相应的站点，在我们的总分发机上创建相应的站点仓库，同时配置我们的Nginx集群对站点进行反向代理配置，如果需要缓存则同步配置SQUID集群。![](/assets/practice-create-web.png)

## 异地容灾：

可以实现生产机房与容灾机房的一键切换，修改DNS记录（操作BIND），修改各WEB节点上的HOSTS，修改DB的主从状态，修改SQUID的HOSTS指向，重启WEB节点。![](/assets/practice-rongzai.png)

## 研发同学查看线上WEB节点的相关信息

研发有时候需要查看线上WEB节点的部分数据，我们基于ELVE将此部分数据展示给研发同学

**获取目标机器的HOSTS信息**![](/assets/practice-get_node_info1.png)

**获取目标机器的配置文件信息**![](/assets/practice-get_node_info2.png)

**查看目标机器的版本是否与分发机一致**![](/assets/practice-get_node_info3.png)

## 其他

整套TRON的体量还是比较庞大的，除了这些功能我们还有

**HOSTS对比与同步操作：**![](/assets/practice-get_node_info4.png)

**SQUID机器HOSTS对比与同步操作**![](/assets/practice-get_node_info5.png)

**等等**

