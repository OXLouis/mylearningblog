# Docker 介绍
## Docker 是什么？
Docker是一个虚拟环境容器，可以将你的开发环境、代码、配置文件等一并打包到这个容器中，并发布和应用到任意平台中。比如，你在本地用Python开发网站后台，开发测试完成后，就可以将Python3及其依赖包、Flask及其各种插件、Mysql、Nginx等打包到一个容器中，然后部署到任意你想部署到的环境。
## Docker 的三个概念
1. 镜像（Image）：类似于虚拟机中的镜像，是一个包含有文件系统的面向Docker引擎的只读模板。任何应用程序运行都需要环境，而镜像就是用来提供这种运行环境的。例如一个Ubuntu镜像就是一个包含Ubuntu操作系统环境的模板，同理在该镜像上装上Apache软件，就可以称为Apache镜像。
2. 容器（Container）：类似于一个轻量级的沙盒，可以将其看作一个极简的Linux系统环境（包括root权限、进程空间、用户空间和网络空间等），以及运行在其中的应用程序。Docker引擎利用容器来运行、隔离各个应用。容器是镜像创建的应用实例，可以创建、启动、停止、删除容器，各个容器之间是是相互隔离的，互不影响。注意：镜像本身是只读的，容器从镜像启动时，Docker在镜像的上层创建一个可写层，镜像本身不变。
3. 仓库（Repository）：类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方。注意与注册服务器（Registry）的区别：注册服务器是存放仓库的地方，一般会有多个仓库；而仓库是存放镜像的地方，一般每个仓库存放一类镜像，每个镜像利用tag进行区分，比如Ubuntu仓库存放有多个版本（12.04、14.04等）的Ubuntu镜像。

## Docker 基本操作
* docker search centos    # 查看centos镜像是否存在
* docker pull centos    # 利用pull命令获取镜像
* docker images    # 查看当前系统中的images信息
* docker ps #列出当前运行的容器 , 加上-a 显示全部
* docker commit : 通过容器导出镜像

    Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
* docker run: 通过镜像产生容器
    
    Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
* docker build: Build an image from a Dockerfile
    
    Usage:  docker build [OPTIONS] PATH | URL | -
    
    -t用来指定新镜像的用户信息、tag等
* docker rm: Remove one or more containers
        
    Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]
* docker rmi: Remove one or more images
        
    Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]
* Dockerfile可以理解为一种配置文件，用来告诉docker build命令应该执行哪些操作。
* docker start container_name/container_id
* docker stop container_name/container_id
* docker restart container_name/container_id
* docker inspect : Return low-level information on Docker objects
        
    Usage:  docker inspect [OPTIONS] NAME|ID [NAME|ID...]

* docker tag : docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]

    相当于给一个旧标签指向的镜像贴上新标签

### Docker 中关于容器的基本操作
* docker run 命令常用参数：<br>
    -i : 表示打开并保持stdout<br>
    -t : 表示分配一个终端<br>
    -d：后台运行。使这个容器处于后台运行的状态，不会对当前终端产生任何输出，所有的stdout都输出到log

### Docker 中关于仓库的基本操作
Docker官方维护了一个DockerHub的公共仓库，里边包含有很多平时用的较多的镜像。除了从上边下载镜像之外，我们也可以将自己自定义的镜像发布（push）到DockerHub上。
<br>
上传镜像步骤
1. 利用 docker login 登录 DockerHub
2. 利用 docker push 将本地的镜像推送到远程的DockerHub上。
    注意：只有路径名等于username的push请求才能推送成功。

## Dockerfile 指令详解
### From指定基础镜像
所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。就像我们之前运行了一个 nginx 镜像的容器，再进行修改一样，基础镜像是必须指定的。而 FROM 就是指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。
### RUN 执行命令
RUN 指令是用来执行命令行命令的。由于命令行的强大能力，RUN 指令在定制镜像时是最常用的指令之一。其格式有两种：
*  shell 格式：RUN <命令>，就像直接在命令行中输入的命令一样。刚才写的 Dockerfile 中的 RUN 指令就是这种格式。
* exec 格式：RUN ["可执行文件", "参数1", "参数2"]，这更像是函数调用中的格式。

## 数据管理
### 数据卷
 数据卷 是一个可供一个或多个容器使用的特殊目录，它绕过 UFS，可以提供很多有用的特性：

* 数据卷 可以在容器之间共享和重用

* 对 数据卷 的修改会立马生效

* 对 数据卷 的更新，不会影响镜像

* 数据卷 默认会一直存在，即使容器被删除

#### 数据卷的创建及查看

        docker volume create my-vol
        docker volume ls
        docker volume inspect my-vol

#### 启动一个挂载数据卷的容器
        docker run --mount source=my-vol,target=xxx xxx 
#### 删除数据卷

        docker volume rm my-vol

数据卷 是被设计用来持久化数据的，它的生命周期独立于容器，Docker 不会在容器被删除后自动删除 数据卷，并且也不存在垃圾回收这样的机制来处理没有任何容器引用的 数据卷。如果需要在删除容器的同时移除数据卷。可以在删除容器的时候使用 docker rm -v 这个命令。
### 挂载主机目录
使用 --mount 标记可以指定挂载一个本地主机的目录到容器中去。

        $ docker run -d -P \
        --name web \
        # -v /src/webapp:/opt/webapp \
        --mount type=bind,source=/src/webapp,target=/opt/webapp \
        training/webapp \
        python app.py

--mount 标记也可以从主机挂载单个文件到容器中

## 使用网络
### 外部访问容器
容器中可以运行一些网络应用，要让外部也可以访问这些应用，可以通过 -P 或 -p 参数来指定端口映射。

当使用 -P 标记时，Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口。

使用 docker container ls 可以看到，本地主机的 49155 被映射到了容器的 5000 端口。此时访问本机的 49155 端口即可访问容器内 web 应用提供的界面。

### 容器互联
使用 Docker 网络来连接多个容器

#### 新建网络

        docker network create -d bridge my-net
-d 参数指定 Docker 网络类型，有 bridge overlay。其中 overlay 网络类型用于 Swarm mode，在本小节中你可以忽略它。

#### 连接容器
通过 --network my-net 将容器连接到网络中。
    
## Services
### services简介
services 就是 “生产中的容器”。 一个service 只对应一个镜像，但是它编码了镜像的运行方式——决定用哪一个端口，产生多少个容器可以满足service的需要。services中每一个单独的容器叫做task<br>
放缩（放大或缩小）一个service可以通过改变容器数量，改变分配资源数量实现。
<br>
services 可以部署在单机上也可以部署在swarm上
### 配置docker-compose.yml文件
    services 通过 这个文件 配置 多个容器构成的集群。
    过程见：[Get Started, Part 3: Services](https://docs.docker.com/get-started/part3/#docker-composeyml)
### services相关简单操作
    docker stack ls                                            # List stacks or apps
    docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
    docker service ls                 # List running services associated with an app
    docker service ps <service>                  # List tasks associated with an app
    docker inspect <task or container>                   # Inspect task or container
    docker container ls -q                                      # List container IDs
    docker stack rm <appname>                             # Tear down an application
    docker swarm leave --force      # Take down a single node swarm from the manager

## Swarms
 Swarm 是加入了一个集群集体运行docker的一组机器。创建集群之后，你仍然使用之前的docker命令，但是这些命令现在由 __swarm manager__ 在集群上执行。swarm中的机器可以是实体机也可以是虚拟机，加入swarm之后，他们被归为 **nodes**。
<br>
Swarm manager 有多种策略调度节点，比如最空节点优先，或者global（保证每一个节点上对每一个镜像都存在一个对应的容器）
manager 是swarm中唯一可以执行命令的机器，他还可以授权其他机器加入swarm成为worker。worker只提供服务但是不会告诉其他机器它能做什么不能做什么。
### 建立swarm
* docker swarm init 打开swarm模式，并使得当前机器成为一个swarm manager
* 通过docker swarm join 命令加入worker
* 通过docker-machine ssh 给虚拟机下达命令
* 通过docker-machine env 命令来配置当前的shell直接在对应的虚拟机上运行命令。这种方式会比ssh更合适因为它允许使用本地的docker-compose.yml文件去部署远端的应用，不需要复制。
### routing mesh
    这个技术使得集群中不管有没有应用副本的节点，都可以通过swarm负载均衡器定位到运行应用副本的节点。
    <br>
    ![](https://docs.docker.com/engine/swarm/images/ingress-routing-mesh.png)
    <br>
    
### stack
stack 是一组依赖相同且互相关联的services，可以一起协作和scale（放缩）。一个stack就足以定义和协调整个应用的所有功能（虽然很多复杂的应用会用到多个stacks）
    


## Docker 其他信息
### Docker 生命周期
1. 开发构建镜像并将镜像push到Docker仓库
2. 测试或者运维从Docker仓库拷贝一份镜像到本地
3. 通过镜像文件开启Docker容器并提供服务

### 目前痛点
1. 软件更新发布及部署低效，过程繁琐且需要人工介入。
2. 环境一致性难以保证。
3. 不同环境之间迁移成本太高。
<br>
    Docker这个虚拟机超级轻量级，仅仅是一个进程而已。与传统的虚拟机比如VM有着巨大的差别，区别看下图：
<br>
![](https://pic4.zhimg.com/v2-70b8e8e12a5a35aa3a55c0cf56b07b8f_b.jpg)

### Docker 优势：
1. 构建容易分发简单
2. 隔离应用解除依赖
3. 快速部署测完就销 

### Docker 容器相对于虚拟机的优点
1. 启动速度快，容器启动本质就是一个开启一个进程而已，因此都是秒启，而 VM 通常要更久。
2. 资源利用率高，一台普通 PC 可以跑成百上千个容器，你跑十个 VM 试试。
3. 性能开销小， VM 通常需要额外的 CPU 和内存来完成 OS 的功能，这一部分占据了额外的资源。

### Docker 完成持续集成、自动交付、自动部署
![](https://pic3.zhimg.com/v2-e071dd3625a1ec6dc62334bbc8862612_b.jpg)
1. RD推送代码到git 仓库或者svn等代码服务器上面，git服务器就会通过hook通知jenkins。
2. jenkine 克隆git代码到本地，并通过dockerFile文件进行编译 。
3. 打包生成一个新版本的镜像并推送到仓库 ，删除当前容器 ，通过新版本镜像重新运行。



## 阅读材料
[如何通俗解释<em>Docker</em>是什么？ - 刘允鹏的回答 - 知乎](https://www.zhihu.com/question/28300645/answer/67707287) <br>
[【 全干货 】5 分钟带你看懂 <em>Docker</em> ！ - 腾讯云技术社区的文章 - 知乎](https://zhuanlan.zhihu.com/p/30713987)
[Docker — 从入门到实践](https://github.com/yeasy/docker_practice)
https://yeasy.gitbooks.io/docker_practice/
