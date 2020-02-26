
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
