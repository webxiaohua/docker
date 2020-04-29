# @author:xh.shen
# @date:2020-03-26
# @desc:制定一个centos7 + nginx1.14.2 + nginx-vts扩展模块
# @link:
# 在工作目录创建package包，并把下载资源置入（链接: https://pan.baidu.com/s/12aDYNinxmcmfUHpYkT1v8w 提取码: 4nfr）
# 设置基础镜像centos
FROM centos:7
# 设置工作目录，并复制文件到/app
WORKDIR /app
COPY package/nginx-1.14.2.tar.gz /app
COPY package/nginx-module-vts-0.1.18.tar.gz /app

# 设置系统时间
RUN mv /etc/localtime /etc/localtime.bak \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 解压nginx、vts
RUN tar -zxvf nginx-1.14.2.tar.gz && tar -zxvf nginx-module-vts-0.1.18.tar.gz

# 安装系统库
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
    && rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm \
    && yum -y install libxml2 libxml2-dev libxslt-devel \
    gd-devel \
    perl-devel perl-ExtUtils-Embed \
    GeoIP GeoIP-devel GeoIP-data \
    pcre-devel \
    openssl openssl-devel \
    google-perftools google-perftools-devel \
    gcc gcc-c++ autoconf automake make

# 编译
RUN cd nginx-1.14.2 \
    && ./configure --prefix=/usr/share/nginx --with-http_ssl_module --add-module=/app/nginx-module-vts-0.1.18 \
    && make && make install

# 删除文件
RUN cd /app \
    && rm -rf * \
    && mkdir /var/log/nginx

# 启动nginx
#RUN cd /usr/share/nginx/sbin && ./nginx -g 'daemon off;'

EXPOSE 80
# 测试是否安装成功
ENTRYPOINT ["/usr/share/nginx/sbin/nginx","-g","daemon off;"]
#CMD [ "nginx", "-V" ]