# Mysql的事例 ##

> 1、下载mysql镜像
docker pull mysql
2、创建镜像
docker run --name mysqldb  -e MYSQL_ROOT_PASSWORD=root -d mysql
3、获取被创建容器的ip
docker inspect mysqldb
4、从主机上利用mysql客户端测试能否连接到容器中的mysql服务
mysql -h 172.17.0.2 -u root -p
上面的ip地址是mysqldb容器的ip地址，通过docker inspect mysqldb获取到的   
