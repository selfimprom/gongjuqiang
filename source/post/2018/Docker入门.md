```toml
title = "Docker笔记"
slug = "Docker笔记"
desc = "Docker笔记"
date = "2018-09-19 23:58:28"
update_date = "2018-09-19 23:58:28"
author = ""
thumb = ""
tags = ["tag"]
```
近期空闲时间翻了一下《第一本Docker书》，这本书作为docker入门还是挺不错的，浅显易懂，在看的时候，做了下读书笔记，以便自己后期查阅方便。  
 
###镜像###
镜像式由文件系统叠加而成  

```
查找镜像
  docker search [image_name]
下载镜像
	docker pull [image_name]
运行
	docker run [image_name]
查看本地镜像
	docker images 
删除镜像
	docker rmi [image_id] 
构建docker镜像：
	docker build -t centos_apache:v1  .
```  

***Dockerfile:***   


```
# base image
FROM  centos  

# MAINTAINER  
MAINTAINER  test@gmail.com  

# put xxx.txt file into /user/local/ 
ADD xxx.txt /user/local/   

# running required command
RUN  apt-get install apache  --构建时候执行的命令   

CMD  [“/bin/bash”，“－l”] --运行时执行的执行的命令、可以在docker run 命令中覆盖cmd命令   

ENTRYPOINT  [“/bin/bash”，“－l”] --与CMD 相比，不容易在启动容器时候被覆盖   

EXPOSE  80 --暴露的端口 
```    
***

###容器###

***docker 容器定义可以概括为:***

* 一个镜像文件  
* 一系列的标准操作  
* 一个执行环境  
容器的唯一表示有：短uuid、长uuid、name   
常用命令  
```
1、docker run -d --name=server-db -p 3306:3306 -v /server/mysql-data:/mysql-data centos6.8-mysql /usr/bin/mysql_safe –d 
args：
-v			          		--宿主机的数据库目录/server/mysql-data挂载到server-db上  
centos6.8-mysql       		--镜像，先查找本地是否有，如果没有，从官方库拉取   
/usr/bin/mysql_safe  		--告诉docker 在容器中运行什么命令  
--restart ＝ always、on-failure:5    --重启条件
--name    	--指定容器名字  
-p  	ip：localport：container-port    --端口映射
-d  		--后台运行
-h 		--hostname，设置容器主机名，默认是容器ID 
--volumes-from container_name   --挂载container_name容器的卷  
-i     --Keep STDIN open even if not attached   
-t       --Allocate a pseudo-TTY   

2、日志
docker logs ［container_name］  
args： 
—tail 0      获取最新的日志 
—tail 10     获取最后十条日志 
－f      	类似 tail －f 
－t  		时间戳 
eg： docker logs －ft －tail 0 test 

3、查看容器内的进程信息  
	docker top ［container_name］ 
     
4、查看cpu、内存等统计信息 
	docker stats ［container_name］ 
    
5、在test容器中启动一个新的bash   
	docker exec -t -i test  /bin/bash   
	 
6、获取所有容器的ID   
 	docker ps -a -q 
   删除所有容器
	docker rm -f `docker ps -a -q`   
   显示最后x个容器  
	docker ps -n -x  
     
7、将宿主机/root/test 拷贝到容器／root／目录下  
	docker cp 源 目的地  实现主机和容器间文件互拷 
	eg：docker cp /root/test container:/root/   
	
8、获得更多的容器信息 
  	docker inspect ubuntu 
  	
9、查看容器中80端口对应到宿主机的映射情况
	docker port contianer 80 
 
``` 
*** 
###网络、存储卷###
***docker通过创建网络来实现互通（也可以用链接），通过创建卷来实现数据共享***
创建网络
docker network create app   

查看才创建的网络 
docker network inspect app   

创建容器时候使用刚才创建的网络app 
docker run －d - -net =app - - name db  image_name 

将已有的容器加入已经创建的网络 
docker network connect app container_name   

断开网络 
docker network disconnect app container_name 

###简单的容器编配 Docker Compose###
###分布式服务发现 Consul ###
### Docker的编配和集群 Swarm ###

