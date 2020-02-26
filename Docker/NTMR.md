```
version: '3'
services:
  nginx:
    image: nginx
    restart: always
    ports:
      - 80:80
    links:
      - tomcat:tomcat
    volumes:
      - /usr/local/docker/nginx/html:/usr/local/nginx/html
  tomcat:
    image: tomcat
    restart: always
    expose:
      - 8080
    volumes:
      - /usr/local/docker/tomcat/webapps:/usr/local/tomcat/webapps/ROOT
  mysql:
    image: mysql/mysql-server
    privileged: true
    restart: always
    ports:
      - 3306:3306
    environment:
      - TZ=Asia/Shanghai
      - MYSQL_ROOT_HOST=%
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - /etc/localtime:/etc/localtime
      - /usr/local/docker/mysql/data:/var/lib/mysql
    command:
      --character-set-server=utf8
      --collation-server=utf8_general_ci
      --default_authentication_plugin=mysql_native_password
      --skip-name-resolve
  redis:
    image: redis
    restart: always
    ports:
      - 6379:6379
    volumes:
      ### 设置容器时区与宿主机保持一致
      - /etc/localtime:/etc/localtime:ro
      - /usr/local/docker/redis/data:/data
      - /usr/local/docker/redis/conf:/usr/local/etc/redis
      - /usr/local/docker/redis/log:/var/log/redis
    
```
