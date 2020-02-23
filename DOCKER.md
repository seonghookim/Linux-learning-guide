- [查看linux发行版，内核](#查看linux发行版，内核)
- [查看是否安装wget](#查看是否安装wget)
- [替换阿里云yum源](#替换阿里云yum源)
- [安装docker](#安装docker)
- [安装docker-compose](#安装docker-compose)
- [百度](https://www.baidu.com)
- [启动docker服务](#启动docker服务)
- [Docker镜像加速](#Docker镜像加速)
- [Docker镜像下载](#Docker镜像下载)
- [启动Docker镜像](#启动Docker镜像)
## 查看linux发行版，内核
[root@localhost ~]# cat /etc/redhat-release  #查看版本号  
CentOS Linux release 7.7.1908 (Core)  
[root@localhost ~]# uname -r  
3.10.0-1062.el7.x86_64

## 查看是否安装wget
[root@localhost ~]# rpm -qa | grep wget  
[root@localhost ~]# yum -y install wget #如果没有安装  

## 替换阿里云yum源
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo #下载阿里yum源2  
[root@localhost ~]# yum makecache  #生成仓库缓存  

## 安装docker
[root@localhost ~]# yum install docker -y  

## 安装docker-compose
[docker-compose官网](https://docs.docker.com/compose/install/)  
[root@localhost ~]# sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  
[root@localhost ~]# sudo chmod +x /usr/local/bin/docker-compose  
[root@localhost ~]# docker-compose version  

## docker服务
[root@localhost ~]# systemctl start docker  #启动docker服务  
[root@localhost ~]# systemctl enable docker #开机启动docker服务  
#Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@localhost ~]# systemctl status docker #查看docker服务状态  

## Docker镜像加速
[root@localhost ~]# vi /etc/docker/daemon.json  
{"registry-mirrors": ["http://hub-mirror.c.163.com"]}  
[root@localhost ~]# systemctl restart docker #事后重启docker  

## Docker镜像下载及启动  
到[Docker hub](https://hub.docker.com/)查询镜像。  
#### &nbsp;&nbsp;&nbsp;&nbsp;[nginx](Docker/NGINX.md)&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;[tomcat](Docker/TOMCAT.md)&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;[mysql](Docker/MYSQL.md)&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;[redis](Docker/REDIS.md)&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;[gitlab](Docker/GITLAB.md)  

#### &nbsp;&nbsp;&nbsp;&nbsp;[nginx+tomcat+mysql+redis](Docker/NTMR.md)

## Docker常用命令  
  * 查看镜像
  ```bash
    docker images
  ```
  * 删除镜像
  ```bash
    docker rmi <image_id(前3位即可)>
  ```
  * 查看容器
  ```bash
    docker ps -a
  ```    
  * 停止容器
  ```bash
    docker stop <container_Id(前3位即可)>
  ``` 
  * 删除容器
  ```bash
    docker rm <container_Id(前3位即可)>
  ```   
  * 进入容器
  ```bash
    docker exec -it <container_Name｜container_Id(前3位即可)> /bin/bash
  ``` 
