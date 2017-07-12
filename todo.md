# Todo

* \[**取消，elves-agent\(v0.4.0\)增加authips授权**\] ~~\[Agent\]提供与Scheduler的通讯验证，目前推荐的是用防火墙建立与Schduler的认证关系，此功能上线后可直接使用内置功能验证（基于RSA）~~

* \[**取消，elves-agent\(v0.4.0\)增加authips授权**\] ~~\[Scheduler\]提供与Agent的通讯验证，目前推荐的是用防火墙建立与Agent的认证关系，此功能上线后可直接使用内置功能验证（基于RSA）~~

* \[Cron\]基于Zookeeper的选举实现，避免出现单点问题（PS：目前我们的是单点到没出现过问题）

* \[HeartBeat\]~~~Agent在线数据写入到zookeeper /heartbeat节点中，避免出现单点问题，后期HeartBeat可以多部署，做选举（PS：目前Agent在线数据存放在HeartBeat内存中）~~~

* \[Supervisor\]砍掉系统所需要依赖的FTP和HTTP，如果启用Superviror，此功能将集成进来，提供Agent下载APP包的地址

* \[Supervisor\]提供对外权限设置接口，主要用于与企业CMDB联动配置权限，提供定期同步CMDB权限功能（需要CMDB提供特殊格式数据）。

* \[Openapi\]内置Jetty，去除对Tomcat容器的依赖。

* \[supervisor\]内置Jetty，去除对Tomcat容器的依赖。

* \[app-sdk\]~~提供csharp版本app-sdk~~

* \[app-sdk\]提供golang版本app-sdk

* \[app-sdk\]提供shell版本app-sdk

* \[all-module\]~~增加Zookeeper依赖开关，允许各组件不依赖zookeeper（会有部分功能受限）~~



