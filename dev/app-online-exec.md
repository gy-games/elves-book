# 上线APP与APP执行

当在测试环境或开发环境对APP进行开发完成后，可以将app发布至elves。

# 打包

将已开发的worker进行一个打包为ZIP格式，注意，打包时不要包含worker目录，ZIP文件解压后即为Worker的可执行文件，且文件名以 **{app名称}\_{版本号}.zip** 命名，如果是C/S模式的APP，其中Processor部分需要自己提前进行部署，当然你可以将不同的语言的APP内置在一个APP包内，elves将根据不同操作系统或者根据用户自定义proxy进行程序调用。

**apptest\_1.0.0 app包示例**

![](/assets/elves-app-apptest.png)

# 发布

app的发布需要借助于heartbeat组件，Heartbeat存在两种模式，且发布方式不同，默认介绍详见:[3.5HearBeat](/module/heartbeat.md)。

## **simple模式**

1、将 **{app名称}\_{版本号}.zip **放入HTTP的根目录，测试可以使用[http://ip:port/{app名称}\_{版本号}.zip](http://ip:port/{app名称}_{版本号}.zip) 进行包的下载

2、配置 auth.localAppInfo={'app名称':'版本号'}

3、重启heartbeat

## **supervisor模式**

1、登录supervisor web界面

2、点击添加APP，WEB端上传{app名称}\_{版本号}.zip，并填写app名称与版本号

4、创建APP

3、授权APP允许运行的机器

# 执行

APP上线后，需要执行必须通过API接口进行触发。所有APP均可采用 及时任务\(RT\)，队列任务\(Queue\)，计划任务\(Cron\) 三种模式进行调用，详见 [API](/api.md)

