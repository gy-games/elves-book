# Changelog

## release v0.1.3 \[20171122\]

* \[All Elves-Center Module\] 优化Elves-Center各组件的RabbitMQ信息交互性能
* \[Supervisor\] 允许权限通过接口配置（无WEB操作界面，需要在数据库内定义）
* \[Agent\] 修订Agent在一定条件下会掉线的BUG，更新安装时自启动放置位置，测试WEB界面小更新
* \[Dashbord\] 发布Dashbord组件
* 增加docker部署支持 https://github.com/elves-project/docker

## new feature \[20170828\]

* \[Agent\] 更新安装时自启动放置位置
* \[Supervisor\] 增加根据URL配置（此功能将包含在后期发布的CMDBProxy中）授权信息功能

## bug fix \[20170811\]

* \[Agent\] 针对开发模式优化web dashbord面板，修订agent与heartbeat交互时超时推出的严重BUG

## release v0.1.2 \[20170723\]

* \[All Elves-Center Module\]日志文件路径修改为：{elves模块}/logs 目录下
* \[Openapi\] 内置Jetty，去除对Tomcat容器的依赖
* \[Supervisor\] 内置Jetty，去除对Tomcat容器的依赖

## release v0.1.1 \[20170710\]

* \[All Elves-Center Module\]增加ZK开关，默认设置关闭ZK，（ZK仅用于组件集群部署时使用）
* \[All Elves-Center Module\]开启ZK时，各组件支持自动创建根节点与自身节点，无需再手工创建
* \[HeartBeat\] 当启用ZK组件时，HB的数据将写入ZK下的节点（为多HB数据共享做准备）
* \[Agent\]增加ELves-Center IP认证，可以设置允许的Scheduler IP地址列表（将Thrift生成的代码集成入agent，并对Thrift Go源码进行修改获取来路IP地址）
* \[AppSDK\]增加csharpSDK\(目前只提供app-worker）

## release v0.1.0 \[20170702\]

* \[**Agent\]**代码重构，重新设计WEB DASHBORD，修订miniAPI与elves-center OpenAPI不一致问题
* \[**Schdule**\] 代码重构
* \[**Cron**\] 代码重构
* \[**Queue**\] 代码重构,QUEUE BUG Fix
* \[**OpenAPI**\] 代码重构，修复超时判断BUG
* \[**HeartBeat**\] 代码重构
* \[**Supervisor**\] 代码重构，页面APP更新功能BUG，APP新增BUG
* \[**AppSDK-python**\] proxy更名，内置入sdk,修复参数存在转移字符时报错的BUG



