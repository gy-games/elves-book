# 调试HelloWord示例

本章节以Python的SDK为例，一步一步的说明如何开发一个简单的ElvesApp

## 1、下载或Git Clone "Python SDK"

**Python-SDK包结构**

```
        │
        ├─processor  #Processor 示例包
        │  │  app.py           #Processor入口问题
        │  │  setting.ini      #Processor配置文件
        │  │
        │  ├─logs              #默认日志目录
        │  │
        │  ├─service
        │  │      service.py   #Processor处理方法
        │  │
        │  └─util              #部分工具类
        │
        └─worker     #Worker 示例包
                appcfg.json    #Worker配置文件
                apptest.py     #Worker执行方法
```

## 2、安装elves-agent，部署apps，并开启开发模式

* elves-agent安装详见: [安装:Elves-Agent](/chapter1/an-zhuang-elves-agent.md) 或 [介绍:Elves-Agent](/zu-jian-jie-shao/elves-agent.md)
* 修改elves-agent配置文件./conf/cfg.json

  * apps增加 {"apptest":"1.0.0"}

  * 设置devmode.enabled = true

* 启动elves-agent

* 将worker目录放入 ./apps，并重命名为apptest

## 3、开发模式**调试APP**

### 3.1 及时任务\(RT\)调试Worker

* 打开elves-agent web管理界面，进入"Develop Tools“

* app输入 apptest , func输入helloword , param输入{"my":"toryzen"} ,点击"Generate"，再点击"Run Test"，获取 APP Worker 执行后结果

### 3.2 计划任务\(Cron\)调试Worker

略

### 3.3 计划任务\(Cron\)调试Worker/Processor

略

### 



