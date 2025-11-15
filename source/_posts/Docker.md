---
title: Docker
date: 2025-03-10
tags: Docker
category: Technical stack
---

# Docker学习笔记

## 一、引言

### 1、什么是Docker

- 是一个创建维护管理容器的工具，在容器基础上，进行进一步分装
- Docker技术 vs 传统虚拟化技术
  - 容器内的应用进程直接运行于宿主的内核，且没有进行硬件硬件虚拟，较为轻便
  - 传统虚拟化是虚拟出一整套意昂见后在其上运行一个完整操作系统

### 2、为什么用Docker

- 1）更高效利用资源
  - 执行速度、内存损耗、文件存储速度等都更高效
- 2）更快速的启动时间
- 3）一致的运行环境
- 4）持续交付和部署
- 5）更轻松的迁移
- 6）更轻松的维护和扩展
  - 分层存储、镜像技术

## 二、基本概念

### 1、镜像

#### 1）Docker 镜像

- 一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）
- **不包含** 任何动态数据，其内容在构建之后也不会被改变
- 由一组文件系统组成（由多层文件系统联合组成）

#### 2）分层存储

- 镜像构建会一层层构建，前一层是后一层的基础
- 每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层  
  比如，删除前一层文件的操作，实际不是真的删除前一层的文件，而是仅在当前层标记为该文件已删除。在最终容器运行的时候，虽然不会看到这个文件，但是实际上该文件会一直跟随镜像。
- 每一层构建时要小心，任何额外东西应该在本层构建结束前清理掉
- 分层技术使得构建、复用、定制变得简单，可以在之前构建好的镜像作为基础层，然后进一步添加新的层

### 2、容器

- 容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等
- 实质是进程，容器进程运行于属于自己的独立的 [命名空间](https://en.wikipedia.org/wiki/Linux_namespaces)。因此容器可以拥有自己的 `root` 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。
- 容器内进程是运行在一个隔离的环境里
- 容器也采用分层存储，每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，为容器运行时读写而准备
- 容器存储生存周期随容器消亡而消亡
- 数据卷的生存周期独立于容器
- 容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化
- 所有的文件写入操作，都应该使用 [数据卷（Volume）](https://yeasy.gitbook.io/docker_practice/data_management/volume)、或者 [绑定宿主目录](https://yeasy.gitbook.io/docker_practice/data_management/bind-mounts)，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。

### 3、仓库

#### 1）仓库

- 通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本
- 通过 `<仓库名>:<标签>` 的格式来指定具体是这个软件哪个版本的镜像，没有则以 `latest` 作为默认标签
- 仓库名经常以 *两段式路径* 形式出现  
  比如 `jwilder/nginx-proxy`，前者往往意味着 Docker Registry 多用户环境下的用户名，后者则往往是对应的软件名

#### 2）Docker Registry 公开服务

- 免费上传、下载公开镜像
- 常用的是官方的 [Docker Hub](https://hub.docker.com/)

#### 3）私有Docker Registry

- 可以基于公开的镜像进行搭建

### 4、之间关系

- 仓库顾名思义，可以拉取镜像
- 镜像类似菜谱，固定的内容，根据镜像可以做出容器
- 容器既是最终的成品，根据同一镜像做出的容器不尽相同

## 三、安装

- [安装指南](https://docs.docker.com/get-docker/)

## 四、使用镜像

### 1、获取镜像

#### 1）获取镜像命令

```bash
$ docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

- Docker镜像仓库地址：  
  地址的格式一般是 `<域名/IP>[:端口号]`，默认地址是 Docker Hub (`docker.io`)
- 仓库名：`<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 `library`，也就是官方镜像。

#### 2）运行

```bash
$ docker run -it --rm ubuntu:18.04 bash
```

- `-it`：`-i` 是交互式操作、`-t` 是终端
- `--rm`：容器退出随之将其删除
- `bash`：放在镜像名后的是命令

### 2、列出镜像

#### 1）指令

```bash
$ docker image ls
```

- 列标包含：仓库名、标签、镜像ID、创建时间、所占用空间
- **镜像ID** 是镜像的唯一标识，一个镜像可以对应多个标签

#### 2）镜像体积

- 这里显示和Docker Hub上看到的镜像大小不同
- Docker Hub显示的是压缩后的体积
- 下载后展开的大小显示的是展开后各层占用空间的总和
- 实际镜像硬盘占用空间比列出来的小，因为有些存储结构可以继承、复用
- 可以通过 `docker system df` 便捷查看镜像、容器、数据卷所占用的空间

#### 3）虚悬镜像

- 由于新旧镜像重名，旧镜像名称被取消，变成 `<none>` 的无标签镜像
- 一般是随着官方镜像维护，发布新版本出现
- 一般来说虚悬镜像已经数去价值，可以随意删除

#### 4）中间层镜像

- 为了加速镜像构建、重复利用资源，Docker会利用中间层镜像
- 一般展开只显示顶层镜像，希望显示中间层镜像加 `-a`

#### 5）列出部分镜像

- 命令后加：仓库名、仓库名和标签等
- 支持强大的过滤器参数 `--filter` 或简写 `-f`

  查看某版本之后建立的镜像可以使用：

  ```bash
  $ docker image ls -f since=mongo:3.2
  ```

  查看某版本之前建立的镜像可以使用：

  ```bash
  $ docker image ls -f befor=mongo:3.2
  ```

  如果构建时定义了 LABEL，可以通过 LABEL 过滤：

  ```bash
  $ docker image ls -f label=com.example.version=0.1
  ```

#### 6）以特定格式显示

- `-q`：只显示ID
- 还可以自定义，这里需要用到Go的模板语法

### 3、删除镜像

#### 1）用ID、镜像名、摘要删除

- 可以使用完整ID删除，但一般使用短ID，即一般取前3字符以上足够区分别的镜像即可
- 可以使用镜像名，即 `<仓库名>:<标签>`
- 可以使用摘要删除

#### 2）Untagged和Deleted

- 一个镜像对应多个标签，上述有些实际只是删除某个标签镜像
- 镜像的多层结构让镜像复用变得简单，可能别的镜像依赖当前镜像某一层，这样不会触发删除该层行为，直到没有任何依赖才能真实删除当前层
- 还需要注意容器对镜像的依赖，即使容器没有运行，只要有用这个镜像启动的容器存在，这样不可删除该镜像

#### 3）用 docker image ls 命令来配合

- 可以成批的删除希望删除的镜像  

  删除所有仓库名为 redis 的镜像：

  ```bash
  $ docker image rm $(docker image ls -q redis)
  ```

### 4、利用 commit 理解镜像构成

- 使用 `docker commit` 命令虽然可以比较直观的帮助理解镜像分层存储的概念，但是实际环境中并不会这样使用

[利用commit理解镜像构成](https://yeasy.gitbook.io/docker_practice/image/commit)

### 5、使用Dockerfile定制镜像

[使用Dockerfile定制镜像](https://yeasy.gitbook.io/docker_practice/image/build)

### 6、Dockerfile指令详解

[Dockerfile指令](https://yeasy.gitbook.io/docker_practice/image/dockerfile)

### 7、多阶段构建

### 8、构建多种系统架构支持的Docker支持的镜像

### 9、其他制作镜像方式

## 五、操作容器

### 1、启动

新建并启动：

```bash
$ docker run ubuntu:18.04 /bin/echo 'Hello world
```

启动已终止：

```bash
$ docker container start
```

### 2、守护态运行

不使用 `-d`：

```bash
$ docker run ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
hello world
hello world
hello world
hello world
```

使用则后台运行。使用下面命令查看：

```bash
$ docker container logs [container ID or NAMES]
hello world
hello world
hello world
. . .
```

### 3、终止

```bash
$ docker container stop
```

查看容器：

```bash
$ docker container ls -a
```

### 4、进入

`attach` 命令（不建议使用）：从容器 `exit` 后会导致容器停止。

`exec` 命令：

```bash
$ docker exec -it <ID> bash
```

### 5、导入和导出

- 导出容器快照：`docker export` 命令

  ```bash
  $ docker export <containerID> > ubuntu.tar
  ```

- 导入容器快照：`docker import`

  ```bash
  $ cat ubuntu.tar | docker import - test/ubuntu:v1.0
  ```

  也可通过指定 URL 或者某个目录来导入：

  ```bash
  $ docker import http://example.com/exampleimage.tgz example/imagerepo
  ```

- *注：用户既可以使用 `docker load` 来导入镜像存储文件到本地镜像库，也可以使用 `docker import` 来导入一个容器快照到本地镜像库。这两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。*

### 6、删除

- 删除容器

  ```bash
  $ docker container rm trusting_newton
  ```

- 清理所有处于终止状态的容器

  ```bash
  $ docker container prune
  ```
