# 计划任务\(Cron\)

计划任务将以设定的规则异步的方式发送至Agent端，接口以非阻塞状态执行，本接口只可以管理由Elves-Center端所发起的计划任务，无法管理由Elves-Agent端发起的计划任务。

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [添加计划任务](/api/cron/tian-jia-ji-hua-ren-wu.md) | POST /api/v2/cron/add | openapi,cron,scheduler | supervisor |
| [开启计划任务](/api/cron/kai-qi-ji-hua-ren-wu.md) | POST /api/v2/cron/start | openapi,cron,scheduler | supervisor |
| [停止计划任务](/api/cron/huo-qu-ji-hua-ren-wu-xiang-xi-xin-xi.md) | POST /api/v2/cron/stop | openapi,cron,scheduler | supervisor |
| [获取计划任务列表信息](/api/public/huo-qu-agent-lie-biao-xin-xi.md) | GET /api/v2/cron/list | openapi,cron,scheduler | supervisor |
| [获取计划任务详细信息](/api/cron/huo-qu-ji-hua-ren-wu-xiang-xi-xin-xi.md) | GET /api/v2/cron/detail | openapi,cron,scheduler | supervisor |

---

计划组件使用Quartz实现，并同时使用其cron语法规则。

**表 5.1. Quartz Cron 表达式支持到七个域**

| **名称** | **是否必须** | **允许值** | **特殊字符** |
| :--- | :--- | :--- | :--- |
| 秒 | 是 | 0-59 | , - \* / |
| 分 | 是 | 0-59 | , - \* / |
| 时 | 是 | 0-23 | , - \* / |
| 日 | 是 | 1-31 | , - \* ? / L W C |
| 月 | 是 | 1-12 或 JAN-DEC | , - \* / |
| 周 | 是 | 1-7 或 SUN-SAT | , - \* ? / L C \# |
| 年 | 否 | 空 或 1970-2099 | , - \* / |



