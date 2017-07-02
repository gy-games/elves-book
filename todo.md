# Todo

## Agent

提供与Scheduler的通讯验证，目前推荐的是用防火墙建立与Schduler的认证关系，此功能上线后可直接使用内置功能验证

## Schduler

提供与Agent的通讯验证，目前推荐的是用防火墙建立与Agent的认证关系，此功能上线后可直接使用内置功能验证

## Cron

基于Zookeeper的选举实现，避免出现单点问题（PS：目前我们的是单点到没出现过问题）

## Queue

## OpenAPI

## HeartBeat



## Supervisor

砍掉系统所需要依赖的FTP和HTTP，如果启用Superviror，此功能将集成进来，提供Agent下载APP包的地址

提供对外权限设置接口，主要用于与企业CMDB联动配置权限，提供定期同步CMDB权限功能（需要CMDB提供特殊格式数据）。

## AppSdk



