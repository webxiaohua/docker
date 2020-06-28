FROM alpine:3.11
RUN echo '#!/bin/sh' >> /run.sh && \
    echo 'rm /etc/nginx/conf.d/default.conf' >> /run.sh && \
    echo 'mkdir -p /run/nginx' >> /run.sh && \
    echo '/usr/sbin/nginx' >> /run.sh && \
    echo '/usr/sbin/php-fpm7' >> /run.sh && \
    echo 'tail -f /etc/hosts' >> /run.sh

RUN cd / && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories && \
    apk update  && \
    apk add build-base curl shadow openssh bash libxml2-dev openssl-dev libjpeg-turbo-dev libpng-dev libxpm-dev freetype-dev gd-dev gettext-dev libmcrypt-dev binutils && \
    addgroup www  && \
    adduser -G www -D -s /sbin/nologin www  && \
    apk add nginx && \
    apk add php7 php7-fpm php7-opcache php7-curl php7-gd php7-mbstring php7-mysqli php7-json php7-mcrypt php7-redis php7-pdo  redis  && \
    apk add php7-xmlwriter php7-xmlrpc php7-xmlreader php7-xml php7-tokenizer php7-sysvsem php7-sqlite3  php7-shmop php7-posix  php7-pdo_mysql php7-pcntl php7-ldap  php7-iconv php7-gettext  php7-fileinfo php7-dom php7-ctype php-bcmath php7-simplexml php7-phar && \
    apk del build-base shadow binutils && \
    apk add -U tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    chmod +x /run.sh
#RUN sed -i 's/;date.timezone =/date.timezone =Asia\/Shanghai/g' /etc/php7/php.ini 
EXPOSE  80
ENTRYPOINT ["/run.sh"]