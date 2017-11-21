# 二进制安装

## Windows

* 下载适合操作系统的二进制安装包并解压 ( [下载地址](https://github.com/gy-games/elves/releases) )
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数conf\cfg.json（详见[Elves-Agent](/module/elves-agent.md)）
* 以**管理员模式**打开CDM并进入程序目录
* 执行 control.cmd install 安装elves-agent （需要输入管理员帐号密码）
* 执行 control.cmd start 开启elves-agent
* 若开启了http服务，则访问 http://ip:port (http://127.0.0.1:19880) 查看运行状态

## Linux

* 下载适合操作系统的二进制安装包并解压 ( [下载地址](https://github.com/gy-games/elves/releases) )
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 conf\cfg.json（详见[Elves-Agent](/module/elves-agent.md)）
* 执行chmod +x ./control 增加执行权限
* 执行 ./control start 开启elves-agent
* 若开启了http服务，则访问 http://ip:port (http://127.0.0.1:19880) 查看运行状态

若需要开机启动，则执行./control install

---

# 编译安装

## Windows

* 下载源码包或 git clone  源码包
* 以**管理员模式**打开CDM并进入源码包目录
* 进入源码包安装执行 go get ./... 下载依赖
* 执行control.cmd build 构建
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 conf\cfg.json（详见[Elves-Agent](/module/elves-agent.md)）
* 执行 control.cmd install 安装elves-agent（需要输入管理员帐号密码）
* 执行 control.cmd start 开启elves-agen
* 若开启了http服务，则访问 http://ip:port (http://127.0.0.1:19880) 查看运行状态

## Linux

* 下载源码包或 git clone 源码包
* 进入源码包安装执行 go get ./... 下载依赖
* 执行chmod +x ./control 增加执行权限
* 执行./control build 构建
* 复制并修改配置文件 conf\cfg.example.json -&gt; conf\cfg.json 
* 根据自身需求修改相关参数 conf\cfg.json（详见[Elves-Agent](/module/elves-agent.md)）
* 执行 ./control install 安装elves-agent
* 执行./ control start 开启elves-agen
* 若开启了http服务，则访问 http://ip:port (http://127.0.0.1:19880) 查看运行状态

---

# 目录结构

```
elves-agent
    ├─apps        #ElvesAPP安装目录
    ├─bin         #执行文件，包括elve-agent主程序，windows版本包含 nssm.exe     
    ├─conf        #配置文件目录          
    │      cfg.example.json    #示例配置文件，使用时需要更名为cfg.json
    │      cron.json           #agent cron配置文件
    │      seelog.xml          #agent 日志配置文件
    ├─logs        #默认的agent执行log的文件目录
    ├─public      #elve-agent web dashbord所需要的静态资源
    └─src         #源码，二进制包时没有此目录
```



