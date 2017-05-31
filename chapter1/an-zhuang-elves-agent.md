# 二进制安装

## Windows

* 下载适合操作系统的二进制安装包并解压
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 详见[Elves-Agent](/zu-jian-jie-shao/elves-agent.md)
* 以**管理员模式**打开CDM并进入程序目录
* 执行 control.cmd install 安装elves-agent
* 执行 control.cmd start 开启elves-agent
* 若开启了http服务，则访问 ip:port 查看运行状态

## Linux

* 下载适合操作系统的二进制安装包并解压
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 详见[Elves-Agent](/zu-jian-jie-shao/elves-agent.md)
* 执行chmod +x ./control 增加执行权限
* 执行 control.cmd start 开启elves-agent
* 若开启了http服务，则访问 ip:port 查看运行状态

若需要开机启动，则执行./control install

---

# 编译安装

## Windows

* 下载源码包或 git clone  源码包
* 以**管理员模式**打开CDM并进入源码包目录
* 进入源码包安装执行 go get ./... 下载依赖
* 执行control.cmd build 构建
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 详见[Elves-Agent](/zu-jian-jie-shao/elves-agent.md)
* 执行 control.cmd install 安装elves-agent
* 执行 control.cmd start 开启elves-agen
* 若开启了http服务，则访问 ip:port 查看运行状态

## Linux

* 下载源码包或 git clone 源码包
* 进入源码包安装执行 go get ./... 下载依赖
* 执行chmod +x ./control 增加执行权限
* 执行./control build 构建
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 详见[Elves-Agent](/zu-jian-jie-shao/elves-agent.md)
* 执行 control.cmd install 安装elves-agent
* 执行 control.cmd start 开启elves-agen
* 若开启了http服务，则访问 ip:port 查看运行状态

---

# 目录结构

```
elves-agent
         
```



