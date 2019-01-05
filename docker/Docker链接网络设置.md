# Docker网络链接设置 #

- 1、如果docker主机不需要通过代理连接外网

    则docker的相关命令（如docker search）或docker容器与网络相关的操作都可以正常进行，不需要特殊设置。

- 2、当docker主机 是通过代理才能连接外网时，采用服务方式启动守护进程

如果docker守护进程是通过服务的方式启动的（sudo start docker）

当我们执行如  docker search ubuntu 命令时，会报错
Error response from daemon: Get https://index.docker.io/v1/search?q=ubuntu: dial tcp: lookup index.docker.io on 127.0.1.1:53: read udp 127.0.1.1:53: i/o timeout

而且这时启动的容器，在容器内也无法连接外网。

需要通过设置来完成。

 

- 3、当docker主机 是通过代理才能连接外网时，让docker守护进程可连接外网，非服务启动方式

通过如下方式启动docker守护进程

sudo HTTP_PROXY=http://代理地址:端口 docker daemon

这时执行如  docker search ubuntu 命令时，可以成功。 注意，这并不需要docker主机自己设置代理上网（也就是docker进程没有利用主机设置的代理上网）。

但是正常启动的容器，在容器内也无法连接外网。

 

- 4、当docker主机 是通过代理才能连接外网时，采用服务方式启动

可以修改 /etc/default/docker 配置文件

#If you need Docker to use an HTTP proxy, it can also be specified here.  
   
#export http_proxy="http://127.0.0.1:3128/"

export http_proxy="http://代理地址:端口"

这样采用 sudo start docker方式启动守护进程后

这时执行如  docker search ubuntu 命令时，可以成功。

注意，这并不需要docker主机自己设置代理上网（也就是docker进程没有利用主机设置的代理上网）。

但是正常启动的容器，在容器内也无法连接外网。

 

- 5、怎么让容器通过代理上网

容器本身是一个轻量级的linux系统，我们可以通让主机上网一样设置让其上网。容器上网和让docker守护进程联网没有关系。

方法一：临时联网

在shell界面上设置临时环境变量：  export http_proxy="http://代理ip地址:端口"

如：export http_proxy="http://10.41.70.8:80"

一旦设置正确的环境变量http_proxy，容器就可以正常上网了。

因为是临时的，shell关闭后，环境变量就没了。

方法二：修改主目录下的.bashrc文件，增加两行

http_proxy=http://yourproxyaddress:proxyport
export http_proxy

就是把环境变量http_proxy持久化，但只对该用户登录有效。

注意：容器设置代理 和 docker主机设置代理以及docker守护进程设置代理无关，也就是容器只会使用自己的代理信息上网。