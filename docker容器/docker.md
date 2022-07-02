# 启动docker服务
>`service docker start`
# 镜像

> - ```docker images```:列出本地主机上的镜像。  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/bc908244f64c429e841dd20e063ab3d1.png)
> - `docker pull ubuntu:20.04`:拉取一个ubuntu:20.04镜像。
> - `docker rmi ubuntu:20.04`:删除ubuntu:20.04的镜像。

# 容器

> - ```docker run```:创建一个新的容器并运行一个命令。
> 参数解释(以`docker run -itd  --gpus all --name ubunut  ubuntu:focal /bin/bash`为例)：使用ubuntu:focal镜像创建一个容器。
> -d: 后台运行容器，并返回容器ID；
> -i: 以交互模式运行容器，通常与 -t 同时使用；
> -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
> --gpus all：nvidia-docker的命令，表示使用所有GPU;
> --name ubuntu：表示创建一个叫ubuntu的容器。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/598786ad7aa740748a3f3295635e3490.png)
> - `docker ps [-a]`:显示运行[所有]容器。![在这里插入图片描述](https://img-blog.csdnimg.cn/2970d12e75b84d0d84612bf0d4daabc7.png)
> - `docker start [CONTAINER ID]`：启动一个停止的容器。
> - `docker stop  [CONTAINER ID]`：停止一个容器。
> - `docker restart [CONTAINER ID]`：重启容器。
> - `docker exec -it [CONTAINER ID] /bin/bash`: 进入一个启动的容器。
