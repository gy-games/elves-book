# 信息\(Info\)

基于认证信息获取已有的权限信息，信息类接口依托于supervisor组件，若不开启supervisor组件，则无法使用信息类接口

| **名称** | **METHOD&URI** | **必须依赖组件** | **可选依赖组件** |
| :--- | :--- | :--- | :--- |
| [获取APP信息列表](/api/public/apps.md) | GET /api/v2/info/apps | supervisor | - |
| [获取AGENT列表信息](/api/public/agent-list.md) | GET /api/v2/info/agents | supervisor | - |
| [获取AGENT详细信息](/api/public/agent-detail.md) | GET /api/v2/info/agents/detail | supervisor,heartbeat | cron |



