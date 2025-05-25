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