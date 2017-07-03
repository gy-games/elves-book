# 基于ELVES实现对MYSQL的运维管理

光宇游戏DB自动化运维相关产品，内部代号Duiker，本系统主要对DB进行信息管理，并可以与工单联动实现对业务工单中新建DB的操作以及运维主从切换操作。

实现的功能有：

* Mysql新建实例
* Mysql主从切换

使用模式有：

* 及时任务rt，CANP

## Mysql新建实例

工单系统新建库的单子自动推送至Duiker![](/assets/practice-duiker-1.png)Duiker调用基于Elves实现实例创建![](/assets/practice-duiker-2.png)

## Mysql主从切换

选择响应的DB实基于ELVES实现主从切换![](/assets/practice-duiker-3.png)

