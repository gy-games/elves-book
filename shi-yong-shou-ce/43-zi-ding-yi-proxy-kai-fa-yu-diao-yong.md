# 自定义App-Worker入口

官方开发SDK中已经内置了相应的入口文件。入口文件的引入主要为了更加便捷的App-Worker的开发，入口文件会通过动态加载的方式调用Worker，并将由elves-agent传入的参数格式化且将app-worker输出的参数格式化，当如如果有特殊需要可以自己实现APP的入口，并告知Elves以此入口调用App-Worker。

---

# 默认APP-Worker入口

* ./app/{appname}/app-worker.py \#python版本app入口，运行后将动态加载 ../apps/{appname}/{appname}.py
* ./app/{appname}/app-worker.exe \#csharp版本app入口，运行后将动态加载 ../apps/{appname}/{appname}.dll

**注意：elves-agent在windows环境下将默认选择csharp版本的入口，Linux环境下将默认选择python的入口**

# APP-Worker入口的参数输入

elves-agent采用进程调用的方式调用调用APp-Worker入口文件，执行参数如下

```
入口文件 {appname} {appfunc} {base64(json(appparam))}
```

* 参数1{appname}：APP 指令名称
* 参数2{appfunc}：APP 方法名
* 参数3{appparam}：以Base64进行编码的Json格式APP参数

# APP-Worker入口的参数输出

文件执行执行后，输出以终端打印方式输出，为了方便elves-agent回收执行结果与状态，我们对执行结果与状态进行了格式化处理

&lt;ElvesWFlag&gt;{flag}&lt;/ElvesWflag&gt;

&lt;ElvesWResult&gt;{result}&lt;/ElvesWResult&gt;

最终需要elves-agent获取的参数使用以上ElvesWFlag、ElvesWResult进行格式化elves-agent即得到执行结果

# 直接使用APP-Worker入口文件调试

因为App-Worker入口文件可以直接独立运行，可以以命令行方式\(shell/cmd\)直接按照Proxy的输入参数进行app-worker的调试工作

---

# 自定义APP-Worker入口

如有一些特殊情况如编程语言的变化等不使用内置的app-worker入口，可以进行自定义，自定义自定义入口的输入与默认入口输入一致，存放位置也一致，其中入口类型分为两种，调用参数也不同。

例如：

```
elves-agent
├─apps 
│ ├─myproxyappdemo1      #自定义Proxy的应用包1
│ │ │ myproxyappdemo.py    
│ │ └─myproxy.py            #自定义的入口地址
│ │
│ └─myproxyappdemo2      #自定义Proxy的应用包2
│   │ myproxyappdemo.ll    
│   └─myproxy.exe            #自定义的入口地址
│
└─...
```

1、如myproxy.py实现了入口文件的参数接入与结果处理，因为.py文件无法直接运行，在调用时需要传入其解释器的名称，这样在我们调用API时 proxy 字段需要传入" python\|myproxy.py ", elves-agent接收到自定义proxy指令后，会调用

```
 python ./apps/myproxyappdemo1/myproxy.py myproxyappdemo1 {func} {base64(json(param))}
```

2、如myproxy.exe实现了入口文件的参数接入与结果处理，因为.exe文件可以直接运行，在调用时可以proxy字段直接传入" myproxy.exe ", elves-agent接收到自定义proxy指令后，会调用

```
./apps/myproxyappdemo1/myproxy.exe myproxyappdemo1 {func} {base64(json(param))}
```

**当然，如果使用自定义的入口，你也可以直接将此入口定义为app程序的逻辑处理，这里没有任何限制**

