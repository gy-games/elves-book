# 安装Elves-Center

Elves-Center可以根据自身的情况进行筛选部署，理论上Elves-Center可以部署在Linux与Windows平台，但我们建议还是部署在Linux平台下，Elves-Center的编译需要maven环境支持，Elves-Agent的编译需要Golang环境的支持

## schduler & queue & cron & heartbeat & openapi & supervisor

以上6个组件的安装过程一致

### 二进制安装

* 下载适合操作系统的二进制安装包并解压

* queue,cron,supervisor模块需要将SQL文件（database.sql）导入MySQL

* 复制配置文件 mv conf/conf.properties.example conf/conf.properties

* 修改配置文件./conf/conf.properties （详见各组件介绍 ）

* 执行chmod +x ./control 设置执行权限

* 执行./control build 构建

* 执行./control start 启动

* 执行./control status 查看运行状态

### 编译安装

* 下载源码包或 git clone  源码包

* 执行 chmod +x ./control &&  ./control build   编译源码

* 复制配置文件 mv conf/conf.properties.example conf/conf.properties

* 修改配置文件./conf/conf.properties （详见各组件介绍 ）

* 执行./control start 启动scheduler

* 执行./control status 查看运行状态

---

# dashbord & watcher

略

