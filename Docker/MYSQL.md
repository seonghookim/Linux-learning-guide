
#### 镜像下载
```
docker pull mysql/mysql-server  
```
#### 生成工作目录
```
mkdir /usr/local/docker/mysql -p
cd /usr/local/docker/mysql
mkdir conf data logs -p
```
#### 启动容器
* 使用命令行启动  
  * –-name=mysql：创建的容器名称，此处命名为mysql
  * –-restart=always：容器的重启策略
  * –-privileged=true：挂载宿主机已存在目录后，在容器内对其进行操作，报“Permission denied”，这时以特权方式启动容器，true
  * --character-set-server=utf8：
  * --collation-server=utf8_general_ci：
  * --default_authentication_plugin=mysql_native_password：
  * --skip-name-resolve： 
  * -v：挂载宿主机目录：容器目录
  * -e：设置环境变量，此处配置容器中的mysql的root用户的host和密码
  * -p：端口映射，宿主机端口:容器端口  
  * -d：后台运行容器，并返回容器ID
  * 最后一个mysql/mysql-server是镜像名称
```
docker run --name=mysql --restart=always privileged=true -v /usr/local/docker/mysql/data:/var/lib/mysql -p 3306:3306 -d mysql/mysql-server 
```
* 使用docker-compose.yml文件启动  
  * 确认当前目录  
    [root@localhost mysql]# pwd  
    /usr/local/docker/mysql  
  * 新建docker-compose.yml文件  
    vi docker-compose.yml  
    ```
    version: '2'
    services:
      mysql:
        container_name: mysql
        image: mysql/mysql-server
        privileged: true
        restart: always
        volumes:
          - /etc/localtime:/etc/localtime
          - /usr/local/docker/mysql/data:/var/lib/mysql
          #- /usr/local/docker/mysql/init:/docker-entrypoint-initdb.d/
        ports:
          - 3306:3306
        environment:
          - TZ=Asia/Shanghai
          - MYSQL_ROOT_HOST=%
          - MYSQL_ROOT_PASSWORD=123456
          #- MYSQL_DATABASE: db
          #- MYSQL_USER: user
          #- MYSQL_PASSWORD: 123456
        command:
          --character-set-server=utf8
          --collation-server=utf8_general_ci
          --default_authentication_plugin=mysql_native_password
          --skip-name-resolve
          #--skip-grant-tables
          #--explicit_defaults_for_timestamp=true
          #--lower_case_table_names=1
          #--max_allowed_packet=128M;
          #--sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO"
    ```
  * 启动  
    ```
    docker-compose up -d  
    ```
* 使用Dockerfile文件启动  
  待续...
