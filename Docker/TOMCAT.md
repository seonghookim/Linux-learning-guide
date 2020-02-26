
#### 镜像下载
```
docker pull tomcat  
```
#### 生成工作目录
```
mkdir /usr/local/docker/tomcat -p
cd /usr/local/docker/tomcat
mkdir conf webapps logs -p
```
#### 启动容器
* 使用docker-compose.yml文件启动  
  * 确认当前目录  
    [root@localhost tomcat]# pwd  
    /usr/local/docker/tomcat  
  * 新建docker-compose.yml文件  
    vi docker-compose.yml  
    ```
    version: "2" # 2为普通docker-compose的yml版本。3为集群版本
    services:
      tomcat:
        container_name: tomcat
        image: tomcat
        privileged: true
        restart: always
        #mem_limit: 1024m # 容器内存限制
        #memswap_limit: 1024m # 容器空间限制
        volumes:
          #- /usr/local/docker/tomcat/conf:/usr/local/tomcat/conf  #映配置文件server.xml到容器里
          - /usr/local/docker/tomcat/webapps:/usr/local/tomcat/webapps/ROOT # 容器目录中不写ROOT, 会出现404错误
          - /usr/local/docker/tomcat/logs:/usr/local/tomcat/logs
          # 在docker-compose.yml文件所在目录，新建target目录，将war包拷贝到该目录下
          # 这个在访问的时候，是以test.war的文件名test来访问的
          #- "./target/test.war:/usr/local/tomcat/webapps/test.war"
        ports:
          - 8080:8080
          #- 8009:8009
        #environment:
          #- TZ=Asia/Shanghai
          #- TOMCAT_USERNAME=admin
          #- TOMCAT_PASSWORD=123456
        #links:
          #- mysql:m1 #连接数据库镜像
          #logging: # https://blog.csdn.net/e_anjing/article/details/96865208
          #options:
          #max-size: "1024k"
          #max-file: "5"
    ```
  * 启动  
    ```
    docker-compose up -d  
    ```
