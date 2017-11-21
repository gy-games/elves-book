# API

Elves-Center与开发者的交互均采用RESTful的API方式实现，并集成在其组件OpenApi中，Api采用 [签名方式](/api/sign.md) 认证请求的合法信息。

API包含4大类接口，信息，即时任务，队列任务与计划任务

## **信息\(Info\)**

基于认证信息获取已有的权限信息，信息类接口依托于supervisor组件，若不开启supervisor组件，则无法使用信息类接口

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [获取APP信息列表](/api/public/apps.md) | GET /api/v2/info/apps | supervisor | - |
| [获取AGENT列表信息](/api/public/agent-list.md) | GET /api/v2/info/agents | supervisor | - |
| [获取AGENT详细信息](/api/public/agent-detail.md) | GET /api/v2/info/agents/detail | supervisor,heartbeat | cron |

# 及时任务\(RT\)

及时任务及立即执行的任务，接口收到指令后会以同步方式将指令发送至相应的Agent端，本次请求的接口阻塞直至Agent端返回执行结果后结束

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [执行及时任务](/api/rt/exec.md) | POST /api/v2/rt/exec | openapi,scheduler | supervisor |

# 队列任务\(Queue\)

队列任务将以异步的方式发送至Agent端，接口以非阻塞状态执行，使用队列任务需要1、创建队列，2、添加队列任务，3、提交队列后队列开始进入执行状态，使用方需要另行发起请求获取队列的执行结果。队列任务支持依赖关系。

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [创建队列](/api/queue/create.md) | POST /api/v2/queue/create | openapi,queue,scheduler | supervisor |
| [添加任务项](/api/queue/add-task.md) | POST /api/v2/queue/addtask | openapi,queue,scheduler | supervisor |
| [提交队列](/api/queue/submit.md) | POST /api/v2/queue/commit | openapi,queue,scheduler | supervisor |
| [停止队列](/api/queue/stop.md) | POST /api/v2/queue/stop | openapi,queue,scheduler | supervisor |
| [获取队列结果](/api/queue/result.md) | GET /api/v2/queue/result | openapi,queue,scheduler | supervisor |

# 计划任务\(Cron\)

计划任务将以设定的规则异步的方式发送至Agent端，接口以非阻塞状态执行，本接口只可以管理由Elves-Center端所发起的计划任务，无法管理由Elves-Agent端发起的计划任务。

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [添加计划任务](/api/cron/add.md) | POST /api/v2/cron/add | openapi,cron,scheduler | supervisor |
| [开启计划任务](/api/cron/start.md) | POST /api/v2/cron/start | openapi,cron,scheduler | supervisor |
| [停止计划任务](/api/cron/stop.md) | POST /api/v2/cron/stop | openapi,cron,scheduler | supervisor |
| [获取计划任务详细信息](/api/cron/info.md) | GET /api/v2/cron/info | openapi,cron,scheduler | supervisor |

计划组件使用Quartz实现，并同时使用其cron语法规则。

