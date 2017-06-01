# 内置Proxy与自定义Proxy

Proxy即app的入口，Proxy概念的引入主要为了更加便捷的App-Worker的开发，Proxy会通过动态加载的方式调用Worker，并将由elves-agent传入的参数格式化且将app-worker输出的参数格式化，目前elves-agent内置了python版本的proxy与charp版本的proxy，以上两个版本的proxy都放置在./bin目录下。

---

# 内置的Proxy

* ./bin/agentProxy.py \#python版本Proxy，运行后将动态加载 ../apps/{appname}/{appname}.py
* ./bin/agentProxy.exe \#csharp版本Proxy，运行后将动态加载 ../apps/{appname}/{appname}.dll

**注意：elves-agent在windows环境下将默认选择csharp版本的Proxy，Linux环境下将默认选择python的Proxy**

# Proxy的参数输入

elves-agent调用app的入口均通过Proxy进入

```
代理器 {appname} {appfunc} {base64(json(appparam))}
```

* 参数1{appname}：APP 指令名称
* 参数2{appfunc}：APP 方法名
* 参数3{appparam}：以Base64进行编码的Json格式APP参数

# Proxy的参数输出

proxy执行后，输出以终端打印方式输出，为了方便elves-agent回收执行结果与状态，我们对执行结果与状态进行了格式化处理

&lt;ElvesWFlag&gt;{flag}&lt;/ElvesWflag&gt;

&lt;ElvesWResult&gt;{result}&lt;/ElvesWResult&gt;

最终需要elves-agent获取的参数使用以上ElvesWFlag、ElvesWResult进行格式化elves-agent即得到执行结果

# 直接使用Proxy调试

因为proxy可以直接独立运行，可以以命令行方式直接按照Proxy的输入参数进行app-worker的调试工作

---

# 自定义Proxy

如有一些特殊情况如编程语言的变化等内置的Proxy无法满足开发需求，可以自定义Proxy，自定义Proxy的输入与内置的Proxy一直，但存放位置放在自身的app内，与实际执行的worker脚本保持一致，且Proxy分为两种，调用参数也不同。

例如：

```
elves-agent
├─apps 
│ ├─myproxyappdemo1      #自定义Proxy的应用包1
│ │ │ myproxyappdemo.py    
│ │ └─myproxy.py            #自定义的Proxy
│ │
│ └─myproxyappdemo2      #自定义Proxy的应用包2
│   │ myproxyappdemo.ll    
│   └─myproxy.exe            #自定义的Proxy
│
└─...
```

1、如myproxy.py实现了proxy的参数接入与结果处理，因为.py文件无法直接运行，在调用时需要传入其解释器的名称，这样在我们调用API时 proxy 字段需要传入 python\|myproxy.py , elves-agent接收到自定义proxy指令后，会调用

```
 python ./apps/myproxyappdemo1/myproxy.py myproxyappdemo1 {func} {base64(json(param))}
```

2、如myproxy.exe实现了proxy的参数接入与结果处理，因为.exe文件可以直接运行，在调用时可以直接传入 myproxy.exe, elves-agent接收到自定义proxy指令后，会调用

    ./apps/myproxyappdemo1/myproxy.exe myproxyappdemo1 {func} {base64\(json\(param\)\)}

