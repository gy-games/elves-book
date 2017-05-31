# Scheduler

务调度组件,发送任务指令，回收任务结果并做后续处理。发起任务的指令来源于OpenApi组件与Cron组件，在任务执行后，Sheduler需要根据任务类型不同选择是否通知App-Processor。若需要通知，则需要向Supervisor请求App-processor信息，以便快速调用。



## 修改配置

**./src/main/resource/conf.properties**

