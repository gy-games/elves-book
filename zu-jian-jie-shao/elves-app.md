# Elves-Apps {#toryzen}

Elves-App分为两部分组成，分别为**Worker**与**Processor**，Worker为一个App必须实现的部分，Processor可以根据自身的需求选择是否对其进行实现。

## Worker

Worker运行在Agent端，Agent端触发App-Worker采用进程调用方式，为进一步简化开发人员的工作、格式化参数的输入与格式化执行结果的回收，我们在Agent端引入了一个Proxy的概念，Proxy接收到指令后，将指令转换为方法以及参数并采用动态加载的方式调用App-Worker并将APP执行结构格式化后反馈至Elves。

以Python为例，开发人员只需要确定方法名、参数（Dictionary）实现自身逻辑即可。

**App "apptest" Worker 实现**

```py
  class apptest():
      def helloword(self,param):
          flag = "false"
          try:
              result = param["my"]
              flag = "true"
          except Exception,e:
        result = e
    return flag,result
```

APP的执行后需要返回两个执行结构，分别为flag与result，均为string类型，flag为枚举:true,false用于确定app的执行状态，result为执行反馈结果。

如上述APP，用户只需要告知Elves，我要执行apptest的helloword方法，参数为{"my":"toryzen"}即可得到toryzen的反馈结果，true,toryzen

若我们传入了异常的参数则会收到fasle,{errinfo}，当然这里的{errinfo}不仅限于上述try catch内容，执行错误的方法或者APP同样会收到由Elves-Agent直接抛出的异常信息。

## Processor

Processor的实现稍微复杂了一些，因Worker处理完的结果将直接反馈至Processor，所以Processor需要以Daemon形式运行一个Thrfit Service等待Worker的信息回传，这里我们也已经将Processor的实现进行了封装，开发人员同样只需要处理自身逻辑即可。

Processor的实践用法很多，例如实现一款C/S架构的APP或者实现执行日志的采集，Processor的启用对应Elves任务\(mode\)的P模式，且相应的我们需要在Worker内添加相应配置文件cfg.json。

**cfg.json**

```
{
    "Processor":
    {
        "Commnet"       :   "This Is Processor CFG , Do Not Use For Other",
        "Addr"          :   "127.0.0.1",
        "Port"          :   10010,
        "Timeout"       :   0
    }
}
```

配置文件中定义了Processor的地址及端口以及超时设置，且必须保证其两者间的互通性。

以Python为例，Processor的处理逻辑在./service/servce.py中

**  ./service/servce.py**

```py
class processorThread(threading.Thread):
    def __init__(self, ins, flag, costtime, message):
        threading.Thread.__init__(self)
        self.ins = ins
        self.flag = flag
        self.costtime = costtime
        self.message = message

    def run(self):
    	log.info("processor start !")
        print "---------GET RESULT----------"
        print "Ins=",self.ins
        print "Flag=",self.flag
        print "Costtime=",self.costtime
        print "Message=",self.message
        log.info("processor end !")
```

其中用户可以得到一个完整的指令：ins，worker的执行状态flag，worker的执行时长costtime 毫秒，worker的执行结果message

逻辑可以在run中自行实现

逻辑开发完成后配置配置文件setting.ini

**./setting.ini**

```
[app]
appName = apptest
Ip      = 127.1.1.1
Port    = 20110
```

定义app的名称，提供接收服务的本机的IP及端口号

最后以nohup运行我们的Processor即可等待接收数据的反馈

```bash
nohup python ./app.py &
```



