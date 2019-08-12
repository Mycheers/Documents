docker的三个基本概念：仓库、镜像、容器（镜像和容器之间的关系可以理解为“类和实例”的关系）

（todo）centos使用docker有必要配置direct-lvm

1.拉取镜像

docker pull [选项] [仓库地址[:端口号]/]仓库名:标签

如果未给出仓库地址则默认从官方（Docker Hub）仓库（library）拉取，即官方镜像
           如：docker pull ubuntu:16.04   ## 从官方拉取ubuntu标签为16.04的镜像

进入到指定镜像
docker exec -it 镜像名  bash

2.查看本地镜像

docker image ls [选项]

docker image ls  # 显示所有顶层镜像

docker image ls -a  # 显示包括中间层镜像在内的所有镜像

docker image ls ubuntu  # 列出所有ubuntu镜像

docker image ls ubuntu:16.04  # 列出ubuntu标签为16.04的镜像

docker image ls -f since=ubuntu:16.04 # -f为filter过滤  since指定列出ubuntu:16.04之后建立的镜像  since替换为before即为之前

-q  列出时只显示镜像id

--format "{{.ID}}: {{.Repository}}"  格式化输出将想要显示的字段用{{.字段}}进行填充

--digests  显示镜像描述信息（sha256）

# 查询本地镜像
[work@VM_0_3_centos ubuntu]$ docker image ls --digests
REPOSITORY          TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
ubuntu              16.04               sha256:a218d8dacc99e49cbdc5862d5c6ee105eb40d4e87bec2e4dac37361c30171ce0   52b10959e8aa        6 days ago          115MB
hello-world         latest              sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc   2cb0d9787c4d        7 weeks ago         1.85kB

3.删除本地镜像

docker image rm [选项] <镜像1> [<镜像2>]

删除时可以通过长id、短id（一般选id的前三位以上，足以区分镜像就可以）、仓库名[:标签]、镜像描述都可以

如：docker image rm 52b10959e8aa

       docker image rm 52b10

       docker image rm ubuntu

       docker image rm ubuntu:16.04

       docker image rm node@sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc

ps:执行删除镜像的命令会有两种结果Untagged(取消标记)和Deleted(删除)。

同一个镜像可以有多个标签，当我们执行删除命令的时候会找到所有满足条件的镜像执行取消标签对镜像的进行标记，

当一个镜像没有被标签标记的时候才会真正删除镜像，所有某些删除命令的结果只有Untagged操作。

另外有基于此镜像启动的容器存在（即便容器不在运行状态）时，此容器将不允许被删除。

# 删除命令可以和查询命令配合使用

docker image rm $(docker image ls -q ubuntu)  # 删除所有ubuntu的镜像

docker image rm $(docker image ls -q -f before=ubuntu:16.04)  # 删除所有在ubuntu:16.04之前建立的镜像