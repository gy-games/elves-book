## Elves-Agent

Elves整体采用C/S架构设计，Elves-Agent即为Client，运行在所有被控机中，Elves-Agent与Elves-Center/App-Processor的通讯采用Thrift形式进行，其本身作为ThriftServer接收来自Elves-Center的指令。

# 安装过程

## 编译&安装

```
cd elves-agent
go get ./...
chmod +x control
./control build
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

## 启动

**Windows**

    control.cmd start

**Linux**

    control.cmd start

# 开发模式



# **安全性**

在Elves-Agent的设计中，我们并未对其指令来源进行安全校验，理论上所有实现其Thrift方法的程序都可以调用Elves-Agent，在实际的成产环境中，我们建议与iptables联用以保证其指令来源的合法性

```bash
iptables -I INPUT -p tcp !-s {scheduler ip} --dport {agent port} -j DROP
```



