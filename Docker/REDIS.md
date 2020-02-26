
#### 镜像下载
```
docker pull redis  
```
#### 生成工作目录
```
mkdir /usr/local/docker/redis -p
cd /usr/local/docker/redis
mkdir conf data logs -p
```
#### 启动容器
* 使用docker-compose.yml文件启动  
  * 确认当前目录  
    [root@localhost redis]# pwd  
    /usr/local/docker/redis  
  * 新建docker-compose.yml文件  
    vi docker-compose.yml  
    ```
    version: '2'
    services:
      redis:
        container_name: redis
        image: redis
        #privileged: true
        restart: always
        volumes:
          # 设置容器时区与宿主机保持一致
          - /etc/localtime:/etc/localtime:ro
          - /usr/local/docker/redis/data:/data
          # /usr/local/docker/redis/conf/redis.conf文件默认设置内容如下：
          # requirepass yourpassword
          # bind: 0.0.0.0
          # loglevel notice
          # logfile redis.log
          # notify-keyspace-events Eglx
          - /usr/local/docker/redis/conf:/usr/local/etc/redis
          - /usr/local/docker/redis/log:/var/log/redis
        ports:
          - 6379:6379
          # redis.conf: bind 172.0.0.1 --> bind 0.0.0.0 从而允许远程访问. 如果要使用密码 requirepass 123456
          # bind 0.0.0.0
          # requirepass 123456
          # tty: true
        environment:
          - TZ= Asia/Shanghai
          - LANG=en_US.UTF-8
        # 生成容器后要执行的命令
        #command:
        #- /bin/bash
        #- -c
        #- chown -R root /var/log/redis&& chgrp -R root /var/log/redis&& redis-server /etc/redis/redis.conf --appendonly yes  --requirepass 123456
    ```
  * 启动  
    ```
    docker-compose up -d  
    ```
