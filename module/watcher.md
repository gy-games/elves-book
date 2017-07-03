# Watcher & Dashbord

Watcher实现对Elves-Center的各组件监控：监听Elves-Center中各组件的运行状态，数据的传输等，便于运维Elves与进行Elves指令审计，Watcher的监听方式以RabbitMQ bind \*.\*的方式进行，对Elves-Center的业务处理无影响，并将监听结果存储至MangoDB供后续查询。Dashbord为运维Elves的运维人员提供一套方便的WEB化管理界面

**Watcher&Dashbord正在进行开源改造中，近期发布。**

