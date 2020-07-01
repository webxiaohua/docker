#### 说明：生产模式下使用的php+nginx镜像，启动时需要挂载应用代码目录和nginx配置文件，并且在应用Dockerfile内启动服务
[docker hub 链接](https://hub.docker.com/repository/docker/webxiaohua/php-nginx-publish)

#### 1.构建基础镜像
```
docker build -t php-nginx-pro:1.0
```

#### 2.构建应用镜像
##### php应用目录结构
```
your project/
├── app
├── bootstrap
├── config
├── ...
├── docker
│   ├── Dockerfile
│   ├── start.sh
│   ├── {your project name}.conf
├── ...
```

##### docker/start.sh 文件内容
```
#!/bin/sh
ENV=$1
if [ -n "$1" ]
then
    echo the env is ${ENV} .
else
    ENV=pro
fi

mv /opt/web/www/.env.${ENV} /opt/web/www/.env

/bin/bash /run.sh
```

##### docker/{your project name}.conf 文件内容
```
server {
    listen      80;
    server_name  ******.com;
    root        /opt/web/www/ {your project name}/public;
    index       index.html index.php;
    client_max_body_size 500m;  # 文件上传大小限制
    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        include         fastcgi_params;
    }
}
```

##### docker/Dockerfile 文件内容
```
FROM php-nginx-pro:1.0
COPY ./docker/[your project name].conf /etc/nginx/conf.d/[your project name].conf

COPY ./docker/start.sh /start.sh

WORKDIR /opt/web/www
COPY app/ app/
COPY bootstrap/ bootstrap/
COPY config/ config/
COPY ...
COPY artisan artisan
COPY .env.dev .env.dev
COPY .env.test .env.test
COPY .env.pro .env.pro

EXPOSE  80
ENTRYPOINT ["/start.sh"]
CMD ["pro"]
```

##### 构建镜像
```
docker build -t [your project name]:[tag]
```

#### 3.运行镜像
```
docker run -p 80:80 -d [your project name]:[tag]
```


