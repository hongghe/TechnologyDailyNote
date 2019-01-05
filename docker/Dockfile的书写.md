# Dockerfile的书写 #

```
# version：0.0.1
FROM ubuntu
MAINTAINER XXX "xxx@test.com"
RUN  apt-get update
RUN  apt-get install -y nginx
RUN  echo 'hello, i am a web image'  > /usr/share/nginx/html/index.html
EXPOSE 80

```

>文件的第一行是一条注释，在dockfile文件中，以#开头的表示注释。
当执行dockerfile文件时（如何执行，后面会介绍），其流程如下：
- 1、从基础镜像运行一个容器（第一条FROM指令的参数用于指定一个已经存在的基础镜像，每个dockerfile文件的第一条指令都是FROM）
- 2、在上面创建的容器中，执行一条指令，对容器做出修改（一般指令总会对容器进行一些改变，否则该指令就不需要了）
- 3、执行类似docker commit命令，提交一个新的镜像层（该层的内容就是上面指令造成的变化内容）
- 4、基于刚提交的镜像运行一个新的容器。
- 5、重复2~4步骤，逐个执行dockerfile中的所有指令。
看到这里大家可以有个疑问，为何每条指令都要创建镜像和容器呢，而不是只只在开始创建一次容器，然后基于该容器执行所有指令，完成所有修改后，再提交生成一个镜像呢？
这正是docker镜像分层特点的优势。可以想象一下，采用这种放的好处是，即使因为某种原因导致某条指令失败，但我们还是可以得到一个可用的镜像，然后我们就可以通过该镜像运行一个容器，在该容器中执行失败的指令，从而方便的进行调试，找到失败原因。
下面我们来介绍上面dokcerfile文件中每一行的意义：
- 1、第一行上面已经介绍，以#开头，是注释。
- 2、第二行FROM指令，用于指定基础镜像。是所有dockerfile的第一条命令。因为所有新的镜像都会基于该基础镜像基础上变化来的。
- 3、第三行 MAINTAINER指令，是标识镜像的作者和联系方式（这里是电子邮件），以方便镜像使用者和作者联系。
- 4、第四~六行RUN指令，用于在容器中执行参数指定的命令。上面的例子第4行是更新已经安装的APT仓库，第5行是下载安装nginx包，第6行是生成一个html文件，文件中只包含简单的一句话。
- 5、最后一行EXPOSE指令，告诉docker守护进程，容器的应用将使用指定的端口号（EXPOSE指令的参数，这个例子是80）。

## 构建镜像 ##

> 通过docker build命令运行dockerfile文件，最后生成需要的镜像。命令如：
docker build -t="jene/myweb" .
参数-t指定生成镜像的所属用户名和仓库名，也可以有tag标识（这个例子没写，默认为latest）。 命令的最后点 不是结束符，而是表示Dockerfile文件在当前路径下。也可以指定一个git仓库的地址（只要该地址下有Dockerfile），则会利用git仓库中的dockerfile文件来构建镜像。
执行的过程会详细 输出每条指令执行的详细信息。
构建成功后，我们就可以用 docker images命令查看新构建的镜像，也可以用docker history命令查看新构建镜像的构建历史。

## 创建容器和启动容器中的web服务 ##

> docker run -d -p 80 --name myweb 1311399350/myweb nginx -g "daemon off;"
上面命令创建和运行了容器，并将nginx服务启动了。
上面命令除 -p参数外，其它参数前面文章都介绍过了。-d表示是 容器。 --name指定容器名，这里是myweb。 后面的是镜像名。以及要执行的命令（这里是启动nginx服务）。 -p参数我们下面详细介绍。
下面我们来通过curl工具测试这个web服务是否可用。
1、先进入容器内访问
docker exec -i -t myweb /bin/bash    //进入容器的交互式shell
curl localhost:80   //可能容器中没有curl工具
输出如下内容
hello, i am a web image
2、在主机上访问容器内的服务
首先查看容器内的80端口与主机上的端口的映射关系
运行 docker port myweb 80 显示信息如下
0.0.0.0:32768
这里可以看出，映射的端口是32768.
我们在主机执行：
curl localhost:32768
输出
hello, i am a web image
3、在主机以外的局域网内的其它机器访问
先用ifconfig查看dokcer主机的ip地址，如 192.168.142.138
在其它机器上通过  http://192.168.142.138:32768/ 一样可以访问到 index.html网页。

## 容器端口的配置 ##

- 方法一：自动映射
docker run -d -p 80 --name myweb 1311399350/myweb nginx -g "daemon off;"
上面的 -p 80 ,将在docker主机上随机打开一个端口（可利用docker port命令查看，或者docker ps也能看到，这里是32768）映射到容器中的80端口上。
- 方法二：指定映射
除了自动映射外，还可以指定映射关系，如：
docker run -d -p 80:80 --name myweb 1311399350/myweb nginx -g "daemon off;"
docker port myweb 80
0.0.0.0:80
可以看出，主机上的80端口映射到容器的80端口。
这样指定的方式有好有怀，坏处是，第一无法运行多个同样的容器，第二容易与主机上的应用冲突。好处是端口是已知的。需要小心使用。
- 方法三：公开dockerfile中EXPOSE指令指定的端口
docker run -d -P --name myweb 1311399350/myweb nginx -g "daemon off;"
 利用大些的-P参数，将dockerfile中EXPOSE指令指定的端口（容器内端口）对本地宿主主机公开，并随机绑定到本地宿主主机的端口上。
docker port myweb 80
0.0.0.0:32771
注意： 在docker run命令中通过 -p标记暴露的容器端口，并不一定需要在dockerfile文件中用EXPOSE指令配置。
在dockerfile文件中用EXPOSE指令配置端口，其好处是在docker run命令中可以用-P标记来全部暴露，省去了在docker run命令中用-p来逐个指定。