# Pull Base image
FROM daocloud.io/centos:latest

# MAINTAINER
MAINTAINER leepin <admin@cxsir.com>

# VERSION
ENV NGINX_VERSION=1.10.1
ENV OPENSSL_VERSION=1.0.2h

# RUN
RUN set -x \
  && yum update -y \
  && yum install -y wget gcc gcc-c++ make automake autoconf libtool perl tar bzip2-devel bzip2 openssl openssl-devel zlib zlib-devel \

# Download openssl
  && cd ~ \
  && wget https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz \
  && tar -zxvf openssl-$OPENSSL_VERSION.tar.gz \

# Download and build nginx
  && wget http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
  && tar -zxvf nginx-$NGINX_VERSION.tar.gz \
  && cd nginx-$NGINX_VERSION \
  && ./configure \
    --prefix=/opt/nginx \
    --with-http_stub_status_module \
    --with-http_gzip_static_module \
    --with-stream \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-http_realip_module \
    --with-http_sub_module \
    --with-openssl=/root/openssl-$OPENSSL_VERSION \
  && make \
  && make install 

ADD ../app/nginx/nginx.conf /opt/nginx/conf/nginx.conf

CMD ["/opt/nginx/sbin/nginx","-g","daemon off;"]

EXPOSE 80 443
