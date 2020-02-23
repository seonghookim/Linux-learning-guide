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

## 启动docker服务
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
* [nginx](Docker/NGINX.md)
* 查询镜像
```bash
  docker images
```
* 删除镜像
```bash
  docker rmi <image_name|image_Id(前3位即可)>
``` 
## 启动Docker镜像  
* nginx
  * 生成宿主机工作目录
  ```bash
    mkdir /usr/local/docker/nginx -p
    cd /usr/local/docker/nginx
    mkdir conf html logs -p
  ```
  * 使用docker-compose文件启动容器
  ```bash
    vi docker-compose.yml
    ---docker-compose.yml文件---
    version: '3'
    services:
      nginx:
        container_name: nginx
        image: nginx
        #build: .
        privileged: true
        restart: always
        volumes:
          - /usr/local/docker/nginx/html:/usr/share/nginx/html
          - /usr/local/docker/nginx/conf:/etc/nginx
          - /usr/local/docker/nginx/logs:/var/log/nginx
          #- /usr/local/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf
          #- /data/nginx/conf.d:/etc/nginx/conf.d
          #- ${NGINX_DIR}/logs:/var/log/nginx
        ports:
          - 8089:80
    -------------------------
    
    docker-compose up -d
  ```
  * 使用docker命令行启动容器
    ```bash
      docker run --restart=always privileged=true --name=nginx \  
      -v /usr/local/docker/nginx/html:/usr/share/nginx/html \  
      -v /usr/local/docker/nginx/conf:/etc/nginx \  
      -v /usr/local/docker/nginx/logs:/var/log/nginx \  
      -p 8089:80  \
      -d nginx
    ```
  * 查看容器状态
  ```bash
    docker ps -a
  ```
  * 进入容器
  ```bash
    docker exec -it nginx /bin/bash
  ```  
  * 停止容器
  ```bash
    docker stop nginx
  ```
  * 删除容器
  ```bash
    docker rm nginx
  ```
