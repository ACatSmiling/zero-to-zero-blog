> *`Author: ACatSmiling`*
>
> *`Since: 2024-10-17`*

**`Docker Compose`**：是一个用于定义和运行多容器 Docker 应用程序的工具。它使用 YAML 文件（通常名为`docker-compose.yml`）来配置应用程序的服务、网络和卷等。通过 Docker Compose，可以使用简单的命令一次性启动、停止和管理多个相关的容器，简化了复杂应用程序的部署和管理过程。

## 主要命令

参考：https://yeasy.gitbook.io/docker_practice/compose

- **`docker-compose up`**
  - **功能**：创建并启动配置文件中定义的所有服务容器。它会按照配置文件中的顺序依次拉取镜像（如果镜像不存在本地）、创建容器并启动容器内的服务。如果服务已经在运行，它会重新启动这些服务以应用任何配置的更改。
  - **语法**：
    - `docker-compose up [SERVICE...]`：如果不指定服务名称，则启动所有服务。例如，"docker-compose up web db" 将只启动名为 web 和 db 的服务。
    - `docker-compose -f <file_path_or_name> <command> [options]`：-f 参数可以指定非默认的 docker-compose.yml 文件路径或者文件名，允许使用一个或多个配置文件或者使用不同名称的配置文件来管理容器。例如，"docker-compose -f file1.yml -f file2.yml up -d"。
    - `docker-compose -d <command> [options]`：以守护进程（daemon）模式运行服务，即容器在后台启动，不会占用终端窗口，并且输出也不会直接显示在终端上。例如，"docker-compose up -d"。
- **`docker-compose down`**
  - **功能**：停止并移除由 Compose 文件定义的所有容器、网络、卷和镜像（可选）。它会按照顺序停止容器，然后移除容器、网络和卷等资源。
  - **语法**：
    - `docker-compose down`：停止并移除所有服务的容器、网络和卷。
    - `docker-compose down -v`：除了停止和移除容器和网络，还移除关联的卷。
- **`docker-compose ps`**
  - **功能**：列出由 Compose 文件定义的服务的容器状态。它会显示每个服务对应的容器的名称、状态、端口映射等信息。
  - **语法**：
    - `docker-compose ps [SERVICE...]`：如果不指定服务名称，则列出所有服务的容器状态。
- **`docker-compose top`**
  - **功能**：查看由 Compose 文件定义的服务容器中正在运行的进程信息。它类似于在单个容器中使用`docker top`命令，但可以同时查看多个服务容器的进程信息。
  - **语法**：
    - `docker-compose top`：可以显示每个服务容器内正在运行的进程列表，包括进程 ID、用户、CPU 和内存使用情况等。
- **`docker-compose pull`**
  - **功能**：拉取配置文件中定义的服务所使用的镜像。如果镜像已经存在本地，它会检查是否有更新的版本，并拉取最新的镜像。
  - **语法**：
    - `docker-compose pull`：可以确保服务使用的镜像是最新版本，避免因为使用旧版本镜像而导致的问题。
- **`docker-compose build`**
  - **功能**：构建（或重新构建）配置文件中定义的服务的镜像。它会读取每个服务的 build 配置，从指定的上下文路径中构建镜像。
  - **语法**：
    - `docker-compose build [SERVICE...]`：如果不指定服务名称，则构建所有服务的镜像。
- **`docker-compose start`**
  - **功能**：启动已经存在的由 Compose 文件定义的服务容器。如果容器不存在，它不会创建新的容器，只会启动已经创建过的容器。
  - **语法**：
    - `docker-compose start [SERVICE...]`。如果不指定服务名称，则启动所有服务的容器。
- **`docker-compose stop`**
  - **功能**：停止由 Compose 文件定义的正在运行的服务容器。它会向容器发送停止信号，让容器内的服务正常停止。需要暂停应用程序的服务时，使用这个命令可以优雅地停止容器，避免数据丢失和服务异常中断。
  - **语法**：
    - `docker-compose stop [SERVICE...]`：如果不指定服务名称，则停止所有服务的容器。
- **`docker-compose restart`**
  - **功能**：重新启动由 Compose 文件定义的所有服务容器。它会先停止容器，然后重新启动它们。
  - **语法**：
    - `docker-compose restart`：可以快速重启所有服务，确保服务在出现问题后能够迅速恢复。
- **`docker-compose logs`**
  - **功能**：查看由 Compose 文件定义的服务容器的日志输出。可以查看所有服务的日志，也可以指定特定服务的日志。
  - **语法**：
    - `docker-compose logs`：查看所有服务容器的日志。
    - `docker-compose logs service_name`：查看特定服务的日志。
- **`docker-compose config`**
  - **功能**：验证并查看 Compose 文件的配置信息。它会检查 Compose 文件的语法是否正确，并显示解析后的配置内容。
  - **语法**：
    - `docker-compose config`：帮助开发人员确保 Compose 文件的配置正确无误，避免因为配置错误导致的服务启动失败。
- **`docker-compose exec`**
  - **功能**：在正在运行的服务容器中执行命令。可以进入容器的 shell 环境，或者执行特定的命令进行调试和管理。
  - **语法**：
    - `docker-compose exec [options] SERVICE COMMAND [ARGS...]`：例如，"docker-compose exec service_name bash"，进入指定服务容器的 bash 终端，进行文件查看、配置修改等操作。

## 应用示例

### MySQL

docker-compose.yaml：

```yaml
version: "3.4"

networks:
  apps:
    name: apps
    external: false

services:
    mysql:
        image: mysql:8.0.33
        container_name: mysql
        hostname: zeloud.mysql
        ports:
          - 3306:3306
        volumes:
          - ./mysql/data:/var/lib/mysql
          - ./mysql/conf/mysql.conf.d/mysqld.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf
          - /etc/localtime:/etc/localtime:ro
        environment:
          - MYSQL_ROOT_PASSWORD=123456
          - MYSQL_INNODB_BUFFER_SIZE=1G
          - MYSQL_SERVER_ID=101
        networks:
          - apps
        restart: on-failure:3
```

./mysql/conf/mysql.conf.d/mysqld.cnf（需要在本地目录预先创建）：

```sh
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
#log-error      = /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address   = 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

max_allowed_packet = 32M
lower_case_table_names=1
max_connections=2000
# innodb缓冲池大小，此处设置为512MB. 512 * 1024 * 1024
innodb_buffer_pool_size=536870912

# binlog配置
#log-bin=mysql-bin
#binlog-format=ROW
#server-id=1
#binlog_ignore_db=information_schema,mysql,performance_schema,sys
#expire_logs_days=30

#sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

### Redis

docker-compose.yaml：

```yaml
version: "3.4"

networks:
  apps:
    name: apps
    external: false

services:
    redis:
        image: redis:7.0.11
        container_name: redis
        hostname: zeloud.redis
        ports:
          - 6379:6379
        volumes:
          - ./redis/conf/redis.conf:/usr/local/etc/redis/redis.conf
          - ./redis/data:/data
        # 挂载redis.conf的话，需要指定启动命令中的配置文件路径
        command: redis-server /usr/local/etc/redis/redis.conf
        networks:
          - apps
        restart: on-failure:3
```

./redis/conf/redis.conf（需要在本地目录预先创建）：

```sh
port 6379
requirepass 123456
protected-mode no
daemonize no
appendonly yes
aof-use-rdb-preamble yes
```

### Rabbitmq

docker-compose.yaml：

```yaml
version: "3.4"

networks:
  apps:
    name: apps
    external: false

services:
    rabbitmq:
        image: rabbitmq:management
        container_name: rabbitmq
        hostname: zeloud.rabbitmq
        ports:
          - 5672:5672
          - 15672:15672
        volumes:
          - ./rabbitmq/data:/var/lib/rabbitmq
        environment:
          - "RABBITMQ_DEFAULT_USER=rbmq"
          - "RABBITMQ_DEFAULT_PASS=rbmq"
        networks:
          - apps
        restart: on-failure:3
```

### Nginx

docker-compose.yaml：

```yaml
version: "3.4"

networks:
  apps:
    name: apps
    external: false

services:
    nginx:
        image: nginx:1.23.4-perl
        container_name: nginx
        hostname: zeloud.nginx
        ports:
          - 80:80
          - 8081:8081
        volumes:
          - ./nginx/conf/nginx.conf:/etc/nginx/nginx.conf
          - ./nginx/conf/conf.d:/etc/nginx/conf.d
          - ./nginx/logs:/var/log/nginx
          - ./nginx/html:/apps/html
          - ./nginx/picture:/apps/picture
          - /etc/localtime:/etc/localtime:ro
        ulimits:
          nofile:
            soft: 65536
            hard: 65536
        networks:
          - apps
        restart: on-failure:3
```

./nginx/conf/nginx.conf（需要在本地目录预先创建）：

```sh
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

./nginx/conf/conf.d/default.conf（需要在本地目录预先创建）：

```sh
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /apps/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /apps/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

./nginx/html/index.html（需要在本地目录预先创建）：

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

./nginx/html/50x.html（需要在本地目录预先创建）：

```html
<!DOCTYPE html>
<html>
<head>
<title>Error</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>An error occurred.</h1>
<p>Sorry, the page you are looking for is currently unavailable.<br/>
Please try again later.</p>
<p>If you are the system administrator of this resource then you should check
the error log for details.</p>
<p><em>Faithfully yours, nginx.</em></p>
</body>
</html>
```

命令：

```sh
# 查看配置文件是否有误
$ docker exec -it nginx nginx -t

# 重新加载配置
$ docker exec -it nginx nginx -s reload
```

### MinIO

docker-compose.yaml：

```yaml
version: "3.4"

networks:
  apps:
    name: apps
    external: false
services:
    minio:
        image: minio/minio:RELEASE.2023-07-21T21-12-44Z
        container_name: minio
        hostname: zeloud.minio
        ports:
          - "9000:9000"
          - "9001:9001"
        volumes:
          - ./minio/data:/data
        command: server /data --console-address ":9001"
        environment:
          MINIO_ACCESS_KEY: "admin"
          MINIO_SECRET_KEY: "@admin2023"
          TZ: Asia/Shanghai
        networks:
          - apps
        restart: on-failure:3
```
