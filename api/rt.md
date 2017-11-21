# 及时任务\(RT\)

及时任务及立即执行的任务，接口收到指令后会以同步方式将指令发送至相应的Agent端，本次请求的接口阻塞直至Agent端返回执行结果后结束

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [执行及时任务](/api/rt/exec.md) | POST /api/v2/rt/exec | openapi,scheduler | supervisor |



