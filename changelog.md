# Changelog

## v0.1.1 \[20170710\]
* \[All Elves-Center Module\]增加ZK开关，默认设置关闭ZK，（ZK仅用于组件集群部署时使用）
* \[All Elves-Center Module\]开启ZK时，各组件支持自动创建根节点与自身节点，无需再手工创建
* \[HeartBeat\] 当启用ZK组件时，HB的数据将写入ZK下的节点（为多HB数据共享做准备）
* \[Agent\]增加ELves-Center IP认证，可以设置允许的Scheduler IP地址列表（将Thrift生成的代码集成入agent，并对Thrift Go源码进行修改获取来路IP地址）
* \[AppSDK\]增加csharpSDK(目前只提供app-worker）

## v0.1.0 \[20170702\]

* \[**Agent\]**代码重构，重新设计WEB DASHBORD，修订miniAPI与elves-center OpenAPI不一致问题
* \[**Schdule**\] 代码重构
* \[**Cron**\] 代码重构
* \[**Queue**\] 代码重构,QUEUE BUG Fix
* \[**OpenAPI**\] 代码重构，修复超时判断BUG
* \[**HeartBeat**\] 代码重构
* \[**Supervisor**\] 代码重构，页面APP更新功能BUG，APP新增BUG
* \[**AppSDK-python**\] proxy更名，内置入sdk,修复参数存在转移字符时报错的BUG



