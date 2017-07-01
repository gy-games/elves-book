# Elves-Agent

Elves整体采用C/S架构设计，Elves-Agent即为Client，运行在所有被控机中，Elves-Agent与Elves-Center/App-Processor的通讯采用Thrift形式进行，其本身作为ThriftServer接收来自Elves-Center的指令。

## 依赖

```
github.com/cihub/seelog                      #日志处理
github.com/toolkits/file                     #文件处理
github.com/jakecoffman/cron                  #定时任务
git.apache.org/thrift.git/lib/go/thrift      #thrift服务
github.com/snluu/uuid                        #UUID生成
```

以上依赖的模块已经Fork至github.com/gy-games-libs

# 安装过程

## 编译&安装

```
cd elves-agent
go get ./...

chmod +x control && ./control build    #linux
control.cmd build                      #windows
```

## 配置

```
mv ./conf/cfg.example.json ./conf/cfg.json
```

**cfg.json**

```
{
        "asset": "localhost",        #主机的别称，一般可以设置CMDB资产的ID
        "ip"   : "127.0.0.1",        #主机的IP地址，这里没有做校验，所以需要认真写
        "port" : 11101,              #Agent接收参数的端口
        "heartbeat": {
                "comment": "if this disabled,agent will not install app automatically",
                "enabled" : true,             #heartbeat 开关，若关闭后将无法自动下载或更新apps
                "addr"    : "127.0.0.1",      #heartbeat IP地址
                "port"    : 11102,            #hearbeat  服务端口
                "interval": 60,               #heartbeat 检测周期
                "timeout" : 5                 #heartbeat 超时时间
        },
        "scheduler": {
                "addr": "115.182.1.130",      #scheduler IP地址
                "port": 10101,                #scheduler 服务端口
                "timeout": 5                  #scheduler 超时时间
        },
        "agentcron":{
                "comment": "agent cron,detail cfg check cron.json",
                "enabled": true               #agent自身cron的开关，若关闭后自身cron将不会执行
        },
        "http": {
                "comment": "this is an agent dashbord , not include import info",
                "enabled": true,              #http server 开关，Elves-Agent提供一个Dashbord来查看当前Agent的运行状态
                "listen": ":19880"            #http server 端口
        },
        "appsdownloadaddr": "http://127.0.0.1/",     #app下载路径
        "apps": {                                    #app列表
                "apptest": "2.0"
        },
        "devmode": {                                 #开发模式
                "comment": "dev mode will start a mini http api service for dev only and it only support 'rt' method ,you can define auth_id and auth_key otherwise it will generat randly",
                "enabled": false,
                "authid": "1",
                "authkey": "123456789"
        }
}
```

## 脚本参数

**Windows control.cmd**

```
"build|install|start|stop|restart|status|uninstall"

build : 运行后将执行go build , 最终构建成 bin\elves-agent.exe ,构建前先go get ./... 解决依赖问题
install : windows下的install将借助bin\nssm.exe将elves-agent安装为名为elves-agent的服务，Elves需要以管理员权限运行，这里需要输入管理员账号密码
start : 执行 net start elves-agent
stop  : 执行 net stop elves-agent
restart : 执行 stop & start
status : 查看elves-agent的运行状态
uninstall : 卸载服务 sc delete elves-agent
```

**Linux ./control**

```
build|pack|install|start|stop|restart|status|tail|uninstall

build : 运行后将执行go build , 最终构建成 bin\elves-agent , 构建前先go get ./... 解决依赖问题
pack  : 运行后会将二进制文件打包成一个tar.gz的包，可用于其他机器的直接部署
install : 执行后会将elves-agent追加入/etc/rc.local
start : 以nohup形式启动elves-agent
stop : 关闭elves-agent
restart : 执行 stop & start
status : 查看elves-agent的运行状态
tail : 可以直接以tail方式查看elves-agent日志
uninstall : 运行后将删除/etc/rc.local下的elves-agent启动项
```

# Elves-Agent Web Dashbord

![](/assets/elves-agent-dashbord.png)

开启HTTP服务后，Elves-Agent将开启一个HTTP服务，显示Elves-Agent的运行状况。

Dashbord：显示Agent的基本信息，包括Agent的运行模式，Agent Asset名称，AgentIP地址，Agent启动时间，Heartbeat最后通讯时间，Agent版本；APPs的信息，本Agent已安装的APP以及其版本；Agent 计划任务，包含最后的执行时间

Recent Tasks：最近40个倒序排列的任务，其中Flag仅代表Worker已经成功执行

Recent Errors：最近20个倒序排列的Error级别的错误

Develop Tools ：开发面板，下面会详细讲述一下

# Develop Tools

![](/assets/elves-agent-develop-tools.png)

开启Elves开发莫时候，即可借助Develop Tools进行APP的开发，目前Develop Tools支持支RT模式的任务执行，输入APP名称，Func名称，Timeout（选填），Proxy（选填），Param（选填）点击 Generate 即可看到 URI以及 POST的参数，测试模式的API支持GET方式请求，可以直接点击Run Test按钮执行。

# Agent Cron Task

Agent端默认 内置计划任务，使用Agent端调用的计划任务信息将不反馈至Elves-Center，agent-cron的配置文件位置为./elves-agent/conf/cron.json

```
{
    "QWASZXCVDFERTYGH": {                       #CRON的ID，可以自定义，但不可以重复
        "Flag": "true",                         #CRON状态，true/false
        "Comment": "this is a test!",           #CRON注释
        "App": "apptest",                       #CRON调用的APP
        "Func": "helloword",                    #CRON调用的APP FUNC
        "Param": {                              #CRON调用的APP FUNC PARAM
            "my":"hello toryzen!"
        },
        "Timeout": 0,                           #CRON调用的APP超时时间
        "Proxy": "python|app-worker.py",        #CRON调用的APPPROXY
        "Mode": "P",                            #CRON调用的APP MODE
        "Rule": "*/15 * * * * *"                #CRON RULE
    }
}
```

cron rule 请参考 [https://github.com/jakecoffman/cron](https://github.com/jakecoffman/cron)

# **安全性**

在Elves-Agent的设计中，我们并未对其指令来源进行安全校验，理论上所有实现其Thrift方法的程序都可以调用Elves-Agent，在实际的成产环境中，我们建议与iptables联用以保证其指令来源的合法性

```bash
iptables -I INPUT -p tcp !-s {scheduler ip} --dport {agent port} -j DROP
```



