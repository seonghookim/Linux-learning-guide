
#### 镜像下载
```
docker pull nginx  
```
#### 生成工作目录
```
mkdir /usr/local/docker/nginx -p
cd /usr/local/docker/nginx
mkdir conf html logs -p
```
#### 启动容器
* 使用命令行启动  
```
docker run --name=nginx --restart=always privileged=true -v /usr/local/docker/nginx/html:/usr/share/nginx/html -v /usr/local/docker/nginx/conf:/etc/nginx -v /usr/local/docker/nginx/logs:/var/log/nginx -p 8089:80 -d nginx 
```
* 使用docker-compose.yml文件启动  
  * 确认当前目录  
    [root@localhost nginx]# pwd  
    /usr/local/docker/nginx  
  * 新建docker-compose.yml文件  
    vi docker-compose.yml  
    ```
    version: '2'
    services:
      nginx:
        container_name: nginx
        privileged: true
        restart: always
        volumes:
          - /usr/local/docker/nginx/html:/usr/share/nginx/html
          - /usr/local/docker/nginx/conf:/etc/nginx
          - /usr/local/docker/nginx/logs:/var/log/nginx
          #- ${NGINX_DIR}/logs:/var/log/nginx
        ports:
          - 8089:80
        image: nginx
    ```
  * 启动  
    ```
    docker-compose up -d  
    ```
* 使用Dockerfile文件启动  
  待续...
