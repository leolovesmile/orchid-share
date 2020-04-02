# 创建启动和停止脚本
对centos较熟悉的工程师应该知道，通常，在启动以jar包形式部署的springboot application的时候，会使用类似如下的命令：
```
nohup java -jar hello-world-springboot-application.jar &
```

但是，在不使用docker、k8s或devops工具的情况下，手动维护应用的话，为了更方便的管理应用，我通常会为应用单独创建目录，并创建对应的启动和停止脚本。
- 示例目录结构大致如下：
```
drwxr-xr-x 2 service service    6 Feb 11 18:10 bin
drwxr-xr-x 2 service service   56 Feb 29 09:29 conf
drwxr-xr-x 2 service service   70 Mar 31 00:12 jar
drwxr-xr-x 2 service service 4096 Mar 30 23:57 log
drwxr-xr-x 2 service service   54 Feb 11 14:03 ssl
-rwxr--r-- 1 service service  272 Apr  2 14:33 shutdown.sh
-rwxr--r-- 1 service service  401 Apr  2 14:22 startup.sh
```

- 启动的shell脚本文件大致如下
```bash
#!/bin/bash

# Author: Lan, Hao
# Date: 2020-04-02
# Version: 0.1
# Desc: This script is to startup the XXX springboot backend.

nohup java -jar ./jar/app-0.0.1-SNAPSHOT.jar --spring.config.location=./conf/application.properties >./logs/app.log 2>./logs/error.log & echo $!>./logs/pid

echo "Finished! Check the log ./logs/app.log to see if everything works fine!"
```

- 停止的shell脚本文件大致如下
```
#!/bin/bash

# author: Lan, Hao
# date: 2020-04-02
# version: 0.1
# desc: shutdown the springboot application for xxx backend

test -f ./logs/pid && kill `cat ./logs/pid`  && rm -f ./logs/pid && echo "shutdown finished" || echo "no pid file found!"
```

## 对于上述内容的解释说明
简单的nohu启动的方式带来一个问题就是会生成一个nohup.out文件，特别是如果你的应用没有专门的配置logbak或者log4j等日志框架来控制日志的输出的话，不是很友好。那么可以使用输出重定向来将不同的输出结果放到合适的文件中去。

备注：
> 使用&后台运行程序：  
>     - 结果会输出到终端  
>     - 使用Ctrl + C发送SIGINT信号，程序免疫  
>     - 关闭session发送SIGHUP信号，程序关闭
> 
> 使用nohup运行程序：  
>     - 结果默认会输出到nohup.out      
>     - 使用Ctrl + C发送SIGINT信号，程序关闭  
>     - 关闭session发送SIGHUP信号，程序免疫  

- 输出重定向  
我们知道当执行shell命令的时候会默认为之打开三个文件，每个文件带有对应的文件描述符来供用户使用，分别是：
  - 标准输入，描述符0，对应的文件句柄位置是`/proc/self/fd/0`  
  - 标准输出，描述符1，对应的文件句柄位置是`/proc/self/fd/1`  
  - 错误输出，描述符2，对应的文件句柄位置是`/proc/self/fd/2`  
但是我们可以通过修改文件描述符的指向实现重定向。如果上面我们的启动命令变为：  
```
nohup java -jar hello-world-springboot-application.jar 1>/dev/null 2>&1 &
```
这里就相当于把标准输出重定向到`/dev/null`这个空设备文件，也就是所谓的“黑洞”；同时将错误输出重定向到标准输出，最终也到了“黑洞”。这里，需要注意的是我们使用了`&`符号来引用文件描述符（你想象，如果不使用`&`的话，那么错误输出就会被写入一文件名为1的文件，而非标准输出）。  

- pid文件打印  
在用nohup创建进程时，用shell的特殊变量$!把最后一个后台进程的PID保存到文件中，在结束应用的时候直接使用。  
附：Shell中的内建变量
  - `$$`: Shell本身的PID（ProcessID）  
  - `$!`: Shell最后运行的后台Process的PID  
  - `$?`: 最后运行的命令的结束代码（返回值）  
  - `$-`: 使用Set命令设定的Flag一览  
  - `$*`: 所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。  
  - `$@`: 所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。  
  - `$#`: 添加到Shell的参数个数  
  - `$0`: Shell本身的文件名  
  - `$1`～`$n`: 添加到Shell的各参数值。$1是第1参数、$2是第2参数…  


## springboot (2.0 or later version) regular configuations

```
server.servlet.context-path=/api
```