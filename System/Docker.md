# Docker

* 服务启动
```service docker start```

* 服务停止
```service docker stop```

* 本地容器列表
```docker images```

* 列出volume
```docker volume ls```

* 列出所有在运行的容器信息
```docker ps [OPTIONS]```
    * -a :显示所有的容器，包括未运行的。
    * -f :根据条件过滤显示的内容。
    * --format :指定返回值的模板文件。
    * -l :显示最近创建的容器。
    * -n :列出最近创建的n个容器。
    * --no-trunc :不截断输出。
    * -q :静默模式，只显示容器编号。
    * -s :显示总的文件大小。

* 容器内部打开终端
```docker exec -i -t  [NAMES] /bin/bash```
退出终端
```exit```

[详细介绍](https://www.runoob.com/docker/docker-command-manual.html

# docker-compose

* 运行
```docker-compose up```


* 运行并挂起
```docker-compose up -d```

* 停止
```docker-compose down```

* 删除容器
先删除container（images运行时的的状态）
可以使用命令docker ps来查看正在运行的container，对于已经退出的container，则可以使用docker ps -a来查看

```shell
docker ps -a
docker rm 117843ade696
```

删除容器

```shell
docker images
docker rmi 117843ade696
```

更新容器

```shell
docker-compose pull
docker-compose up -d
```

>使用docker-compose up -d以分离模式启动所有服务(-d)(在分离模式下不会看到任何日志)
>使用docker-compose logs -f -t将自己附加到所有正在运行的服务的日志中,而-f表示您遵循日志输出,而-t选项为您提供时间戳(请参阅Docker reference)
>使用Ctrl z或Ctrl c将自己与日志输出分离,而不关闭正在运行的容器

## .yaml

### version:
定义了版本信息

### services:
> 定义了服务的配置信息，包含应用于该服务启动的每个容器的配置

### image:
容器名称

### ports:

### volumes:
> 挂载一个目录或者一个已经存在的数据卷容器，可以直接使用[HOST:CONTAINER]这样的格式，或者使用[HOST:CONTAINER:ro]这样的格式，或者对于容器来说，数据卷是只读的，这样可以有效保护宿主机的文件系统。
compose的数据卷指定路径可以是相对路径，使用 . 或者 … 来指定性对目录

```本地路径:容器内部路径```


只是指定一个路径，Docker 会自动在创建一个数据卷（这个路径是容器内部的）。
```shell
- /var/lib/mysql
```

**例子:**

```shell
// 只是指定一个路径，Docker 会自动在创建一个数据卷（这个路径是容器内部的）。
- /var/lib/mysql

// 使用绝对路径挂载数据卷
- /opt/data:/var/lib/mysql

// 以 Compose 配置文件为中心的相对路径作为数据卷挂载到容器。
- ./cache:/tmp/cache

// 使用用户的相对路径（~/ 表示的目录是 /home/<用户目录>/ 或者 /root/）
- ~/configs:/etc/configs/:ro

// 已经存在的命名的数据卷。
- datavolume:/var/lib/mysql
```


### volume_driver: mydriver
如果你不使用宿主机的路径，你可以指定一个volume_driver。
> 从其它容器或者服务挂载数据卷，可选的参数是:ro或者:rw，前者表示容器只读，后者表示容器对数据卷是可读可写的，默认是可读可写的

```
- service_name
- service_name:ro
- container:container_name
- container:container_name:rw
```


1. Docker容器的重启策略

Docker容器的重启策略是面向生产环境的一个启动策略，在开发过程中可以忽略该策略。

Docker容器的重启都是由Docker守护进程完成的，因此与守护进程息息相关。

Docker容器的重启策略如下：

no，默认策略，在容器退出时不重启容器
on-failure，在容器非正常退出时（退出状态非0），才会重启容器
on-failure:3，在容器非正常退出时重启容器，最多重启3次
always，在容器退出时总是重启容器
unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
2. Docker容器的退出状态码

docker run的退出状态码如下：

0，表示正常退出
非0，表示异常退出（退出状态码采用chroot标准）
125，Docker守护进程本身的错误
126，容器启动后，要执行的默认命令无法调用
127，容器启动后，要执行的默认命令不存在
其他命令状态码，容器启动后正常执行命令，退出命令时该命令的返回状态码作为容器的退出状态码
3. docker run的--restart选项

通过--restart选项，可以设置容器的重启策略，以决定在容器退出时Docker守护进程是否重启刚刚退出的容器。

--restart选项通常只用于detached模式的容器。

--restart选项不能与--rm选项同时使用。显然，--restart选项适用于detached模式的容器，而--rm选项适用于foreground模式的容器。

在docker ps查看容器时，对于使用了--restart选项的容器，其可能的状态只有Up或Restarting两种状态。

示例：
docker run -d --restart=always ba-208
docker run -d --restart=on-failure:10 ba-208
 

补充：

查看容器重启次数
docker inspect -f "{{ .RestartCount }}" ba-208
查看容器最后一次的启动时间
docker inspect -f "{{ .State.StartedAt }}" ba-208

