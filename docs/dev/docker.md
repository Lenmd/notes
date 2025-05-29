## 基础

### docker 命令
```
# 查看镜像
docker images 或 docker image ls

# 查看容器
docker ps 或 docker container ls

# 查看所有容器（包括已停止的）
docker ps -a 或 docker container ls -a

# 删除已停止的容器
docker container prune

```

### 安装MySQL

docker
```
### 拉取MySQL镜像
sudo docker pull mysql:8.4.3

### 创建数据持久化目录（推荐）
sudo docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d -p 3306:3306 -v /data/mysql/data:/var/lib/mysql mysql:8.4.3

### 进入容器
sudo docker exec -it mysql -uroot -p

```


### 安装Redis
```
docker run -d \
  --name nginx80 \
  -p 80:80 \
  -v $(pwd)/nginx/html:/usr/share/nginx/html \
  -v $(pwd)/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
  -v $(pwd)/nginx/conf/conf.d:/etc/nginx/conf.d \
  nginx
```

### docker compose

```
# 启动
docker compose up
-d 后台启动

# 停止
docker compose down

```

nginx docker-compose-yml
``` yaml
services:
  nginx-web:
    image: nginx:1.28.0
    container_name: nginx-web
    network_mode: host  # 主机模式，容器可以直接访问到本机的端口服务
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./log:/var/log/nginx
```

mysql docker-compose-yml
```
services:
  mysql:
    image: mysql:8.0.33
    container_name: mysql3306
    environment:
      # 时区上海
      TZ: Asia/Shanghai
      # root 密码
      MYSQL_ROOT_PASSWORD: 123456
      # 初始化数据库(后续的初始化sql会在这个库执行)
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
    volumes:
      # 数据挂载
      - ./mysql/data/:/var/lib/mysql/
      # 配置挂载
      - ./mysql/conf/:/etc/mysql/conf.d/
    command:
      # 将mysql8.0默认密码策略 修改为 原先 策略 (mysql8.0对其默认策略做了更改 会导致密码无法匹配)
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
    privileged: true
```

redis docker-compose-yml
```
services:
	redis:
    image: redis:6.2.12
    container_name: redis6379
    ports:
      - "6379:6379"
    environment:
      # 时区上海
      TZ: Asia/Shanghai
    volumes:
      # 配置文件
      - ./redis/conf:/redis/config:rw
      # 数据文件
      - ./redis/data/:/redis/data/:rw
    command: "redis-server /redis/config/redis.conf"
    privileged: true
```

minio docker-compose-yml
```
services:
	minio:
    image: minio/minio:RELEASE.2023-04-13T03-08-07Z
    container_name: minio
    ports:
      # api 端口
      - "9000:9000"
      # 控制台端口
      - "9001:9001"
    environment:
      # 时区上海
      TZ: Asia/Shanghai
      # 管理后台用户名
      MINIO_ROOT_USER: testuser
      # 管理后台密码，最小8个字符
      MINIO_ROOT_PASSWORD: 123456
      # https需要指定域名
      #MINIO_SERVER_URL: "https://xxx.com:9000"
      #MINIO_BROWSER_REDIRECT_URL: "https://xxx.com:9001"
      # 开启压缩 on 开启 off 关闭
      MINIO_COMPRESS: "off"
      # 扩展名 .pdf,.doc 为空 所有类型均压缩
      MINIO_COMPRESS_EXTENSIONS: ""
      # mime 类型 application/pdf 为空 所有类型均压缩
      MINIO_COMPRESS_MIME_TYPES: ""
    volumes:
      # 映射当前目录下的data目录至容器内/data目录
      - ./minio/data:/data
      # 映射配置目录
      - ./minio/config:/root/.minio/
    command: server --address ':9000' --console-address ':9001' /data  # 指定容器中的目录 /data
    privileged: true

```