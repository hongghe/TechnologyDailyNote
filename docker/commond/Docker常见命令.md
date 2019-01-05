# Docker中常见命令的使用 #

## 常见命令 ##

> docker run -p 80:80 -d  docker.io/nginx
docker cp index.html containerId://usr/share/nginx/html
docker exec -it containeId /bin/bash
docker images
docker ps [-a -q]
docker stop containerId
docker rm containerId
docker rmi imagesId
docker commit –m ’msg’ containerId [name] 
docker build
docker pull
docker push
docker login

## 创建交互容器 ##

> docker run -i -t ubuntu/bin/bash   
> 其中-i -t 表示创建一个提供交互式shell的容器。
ubuntu是镜像名，如果本地不存在，回到仓库中下载。
/bin/bash 是指定容器创建后立即执行的命令。
注意:每个容器都有一个唯一的ID，作为容器的标识。每个容器也有个唯一的名称，在用docker run命令创建时可以通过 --name 名称 来指定，如果不指定，系统会自动产生一个名称。
如： docker run --name  mycontainer   -i -t ubuntu /bin/bash
对于交互式容器，当退出shell后，容器会关闭。 后面可以通过命令重新启动容器。


## 启动和停止容器 ##

> 如果一个容器已经停止，可以执行如下docker start命令重新启动容器，参数可以是容器的ID 或容器的名称。
docker start 3d72d0283dc8
执行后返回容器的ID
注意，如果指定的容器已经处于启动状态，上述命令只是返回容器ID，不会重新启动容器。
如果要想重启已经启动的容器，可以用 docker restart命令
如果要停止一个运行的容器，可以用 docker stop命令，kill命令也可停止容器，但这命令时强制立即停止容器。

## 附着到交互式容器上 ## 

> 当重新启动容器时，会沿用创建容器（docker run）命令时指定的参数来运行。如果创建容器时，指定了shell。
重启容器时，可以用 docker attach命令附着到容器上，当执行docker attach命令时（可能需要敲下回车键），就回到了容器的bash提示符，
这时就已经相当于在容器内部了的shell操作了。如果操作过程中，退出了shell。容器也会随之停止。
所以这种容器一般是完成特定任务的，不适合运行服务程序。

## 创建守护式容器 ## 

> 这种容器指容器可以长期一直运行，没有交互式会话，非常适合容器中运行后台应用程序和服务（如数据库服务、web服务器等）。
 例子：docker run --name mydaemon -d ubuntu /bin/sh -c "while true;do echo hello world;sleep 1;done"
上述语句利用-d标识创建了一个守护式容器，该容器启动了一个shell，循环打印一个信息，保证shell不退出。
可以通过docker logs命令来获取容器的日志
还可以通过 docker top 命令来查看容器内当前运行的进程信息。

## 与守护式容器交互 ## 

> 可以通过docker exec命令在容器内部额外启动新进程。
如在主机中，执行语句 docker exec -t -i mydaemon /bin/bash
则会出现一个shell会话（容器内的，不是主机的），这样就可以和容器进行交互了，可以完成自己想要的操作。

## 查看容器详细信息 ##

> 利用docker logs 命令可以获取容器的日志信息。
利用docker inspect 命令可以查看容器更多的信息。 如ip地址等，这对守护容器还是很有意义的。

## 执行容器内命令 ##

> 可以在docker主机上，执行 docker exec命令， 在容器内部启动新的进程。
比如：docker exec -i -t  容器ID/NAME  /bin/bash
上面命令表示在容器内打开一个shell交互式会话，参数 -i -t 是让这个shell能背主机捕捉到，可以在主机上操作该shell。通过这个命令，就可以对容器进行相关的操作了，如进行容器的配置、应用程序的配置等。
注意：这个方式和attach不同。attach绑定的shell退出后容器会退出。这种方式不会。

## 删除容器 ##

> 命令：docker rm ID/NAME
注意，运行中的容器是无法被删除的。
注意：在利用docker run创建容器时，可以加上标识 --rm，会在容器运行完毕后，自动删除容器，相当于创建的是一个一次性容器。如：
docker run --rm  .......

## 查看容器的内容改变信息 ## 

> 创建一个容器，会在容器的对应的镜像上增加一个可写层，镜像部分是只读的。通过 diff命令可以看出改变的信息。如：   

 ```

 docker diff mysqldb
A /hello
C /root
A /root/.bash_history
A /root/.mysql_history
C /run
C /run/mysqld
A /run/mysqld/mysqld.pid
A /run/mysqld/mysqld.sock
A /run/mysqld/mysqld.sock.lock
C /tmp
 ```

 > 说明：每行代表一个变动的文件或目录。其中 A 表示新增、C表示被修改、D表示被删除（这个例子没有体现）。   

 ## 主机和容器之间的文件拷贝 ## 


 > 很多场景下，我们需要从主机将文件拷贝到容器中，或从容器拷贝文件都主机。利用 cp命令即可，语法格式如下：
docker cp  主机路径    ID/NAME:容器路径        //这是从主机拷贝文件到容器
docker cp   ID/NAME:容器路径    主机路径        //这是从容器拷贝文件到主机

## 创建容器 ##

> 我们上面的介绍都是用 docker run 创建容器，并在创建成功后立即启动该容器。
还有另外一个docker create命令，该命令使用格式同run命令，但它只创建容器，不会立即启动。要想运行容器，需要单独再执行启动命令。
需要注意的是，使用docker create创建守护容器时，不能带-d标识符。
实际上无论是 run ，还是create命令，都有大量可选的参数，我们这里只是介绍最基本的使用方式（也就是说使用尽量少的参数）。在实际生产环境中，往往会更加复杂。

## 重命名容器 ##

> 每个一个容器除了ID外，都有一个name（可以在创建时指定，也可以不指定，由系统自动分配）。
当容器创建后，也可以通过rename命令给容器重命名。重命名时，容器处于运行或停止状态都允许修改。
语法格式： docker rename oldname newname
这个命令还是挺有用的，当一个容器的name不适合时，就需要重新创建，只需修改名称即可。


## 镜像的复制 ##

> 一个镜像是属于一个仓库，一个仓库中有多个镜像，大家靠tag来区分。
在某些场景下，可能需要把一个已有的镜像 加入（也就是复制）到别的仓库中。这时可以用tag命令。具体的语法格式是：
docker tag [OPTIONS] orignIMAGE[:TAG] [REGISTRYHOST/][USERNAME/]newNAME[:TAG]
这个还是挺有用的，比如当创建一个镜像，命名不适合（仓库名和TAg标识），这样相当于改个名，但实际是拷贝一份。


## 查看主机下载的镜像 ##

> docker images


## 查看主机创建的容器 ##

> docker ps -a -q    

> 说明 -a表示列出所有容器（包括停止运行的容器），否则只会列出运行中的容器。 -q表示只返回容器ID信息，其它容器信息（如状态、对应的镜像等）不显示。

## 列出主机上已经创建的容器 ## 

> docker ps -a