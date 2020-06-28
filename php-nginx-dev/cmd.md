#### 说明：开发模式下使用的php+nginx镜像，启动时需要挂载应用代码目录和nginx配置文件

##### 1.构建镜像
```
docker build -t php-nginx-dev:1.0
```

##### 2.运行镜像
```
docker run --name php-nginx-dev -p 80:80 -d \
-v [your workspace with php code]:/opt/web/www \
-v [your nginx host config]:/etc/nginx/conf.d/[app name].conf \
php-nginx-dev:1.0
```
或者采用 docker-compose 方式运行
```
docker-compose up -d
```

