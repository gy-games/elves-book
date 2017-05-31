# Elves-Agent

Elves整体采用C/S架构设计，Elves-Agent即为Client，运行在所有被控机中，Elves-Agent与Elves-Center/App-Processor的通讯采用Thrift形式进行，其本身作为ThriftServer接收来自Elves-Center的指令



## 安全性

在Elves-Agent的设计中，我们并未对其指令来源进行安全校验，理论上所有实现其Thrift方法的程序都可以调用Elves-Agent，在实际的成产环境中，我们建议与iptables联用以保证其指令来源的合法性

    iptables -I INPUT -p tcp  !-s {scheduler ip} --dport {agent port} -j DROP



