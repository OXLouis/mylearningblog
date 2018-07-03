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

### Docker 中关于容器的基本操作
* docker run 命令常用参数：
    -i : 表示打开并保持stdout
    -t : 表示分配一个终端
    -d：后台运行。使这个容器处于后台运行的状态，不会对当前终端产生任何输出，所有的stdout都输出到log

### Docker 中关于仓库的基本操作
Docker官方维护了一个DockerHub的公共仓库，里边包含有很多平时用的较多的镜像。除了从上边下载镜像之外，我们也可以将自己自定义的镜像发布（push）到DockerHub上。
<br>
上传镜像步骤
1. 利用 docker login 登录 DockerHub
2. 利用 docker push 将本地的镜像推送到远程的DockerHub上。
    注意：只有路径名等于username的push请求才能推送成功。

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
