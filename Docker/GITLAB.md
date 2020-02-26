
#### 镜像下载
```
docker pull twang2218/gitlab-ce-zh  
```
#### 生成工作目录
```
mkdir /usr/local/docker/gitlab -p
cd /usr/local/docker/gitlab
mkdir conf data logs -p
```
#### 启动容器
* 使用docker-compose.yml文件启动  
  * 确认当前目录  
    [root@localhost nginx]# pwd  
    /usr/local/docker/nginx  
  * 新建docker-compose.yml文件  
    vi docker-compose.yml  
    ```
    version: "3"
    services:
      gitlab:
        image: twang2218/gitlab-ce-zh
        container_name: gitlab
        restart: always
        privileged: true
        hostname: '192.168.0.100'
        environment:
          GITLAB_OMNIBUS_CONFIG: |
            external_url "http://192.168.0.100:8181"
            gitlab_rails['gitlab_shell_ssh_port'] = 2222
            gitlab_rails['gitlab_email_enabled'] = true
            gitlab_rails['gitlab_email_from'] = 'XXX@qq.com'
            gitlab_rails['gitlab_email_display_name'] = 'root'
            gitlab_rails['gitlab_email_reply_to'] = 'XXX@qq.com'
            gitlab_rails['smtp_enable'] = true
            gitlab_rails['smtp_address'] = "smtp.qq.com"
            gitlab_rails['smtp_port'] = 465
            gitlab_rails['smtp_user_name'] = "XXX@qq.com"
            gitlab_rails['smtp_password'] = "XXX"
            gitlab_rails['smtp_domain'] = "smtp.qq.com"
            gitlab_rails['smtp_authentication'] = "login"
            gitlab_rails['smtp_enable_starttls_auto'] = true
            gitlab_rails['smtp_openssl_verify_mode'] = 'peer'
            gitlab_rails['smtp_tls'] = true
        ports:
          - '8181:8181'
          - '8443:443'
          - '2222:22'
        volumes:
          - '/usr/local/docker/gitlab/conf:/etc/gitlab'
          - '/usr/local/docker/gitlab/logs:/var/log/gitlab'
          - '/usr/local/docker/gitlab/data:/var/opt/gitlab'
    ```
  * 启动  
    ```
    docker-compose up -d  
    ```
