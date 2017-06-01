# 自定义Proxy开发与调用

Proxy概念的引入主要为了更加便捷的App-Worker的开发，Proxy会通过动态加载的方式调用Worker，并将由elves-agent传入的参数格式化且将app-worker输出的参数格式化，目前elves-agent内置了python版本的proxy与charp版本的proxy，以上两个版本的proxy都放置在./bin目录下。

## Proxy的输入

elves-agent调用app的入口均通过Proxy进入

    代理器 {appname} {appfunc} {base64{appparam}}

