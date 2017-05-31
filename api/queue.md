# 队列任务\(Queue\)

队列任务将以异步的方式发送至Agent端，接口以非阻塞状态执行，使用队列任务需要1、创建队列，2、添加队列任务，3、提交队列后队列开始进入执行状态，使用方需要另行发起请求获取队列的执行结果。队列任务支持依赖关系。

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [创建队列](/api/queue/chuang-jian-dui-lie.md) | POST /api/v2/queue/create | openapi,queue,scheduler | supervisor |
| [添加任务项](/api/cron/tian-jia-ji-hua-ren-wu.md) | POST /api/v2/queue/addtask | openapi,queue,scheduler | supervisor |
| [提交队列](/api/queue/ti-jiao-dui-lie-ren-wu.md) | POST /api/v2/queue/commit | openapi,queue,scheduler | supervisor |
| [停止队列](/api/queue/ting-zhi-dui-lie-ren-wu.md) | POST /api/v2/queue/stop | openapi,queue,scheduler | supervisor |
| [获取队列结果](/api/queue/huo-qu-dui-lie-ren-wu-jie-guo.md) | GET /api/v2/queue/result | openapi,queue,scheduler | supervisor |



