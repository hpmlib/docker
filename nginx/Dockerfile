FROM daocloud.io/alpine:latest

MAINTAINER Leepin.du <admin@cxsir.com>

ENV OPENSSL_VERSION 1.0.2h
ENV NGINX_VERSION 1.10.1
ENV TIMEZONE Asia/Shanghai

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
        && apk update \
        && apk upgrade \

        && apk add tzdata \
        && ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
        && echo $TIMEZONE > /etc/timezone \

        && apk --update add wget \
                gcc \
			    libc-dev \
			    make \
			    openssl-dev \
			    pcre-dev \
			    zlib-dev \
			    linux-headers \
			    curl \
			    jemalloc-dev \
			    gd-dev \

        # Download openssl
        && mkdir ~/download \
        && cd ~/download \
        && wget --no-check-certificate https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz \
        && tar -zxvf openssl-$OPENSSL_VERSION.tar.gz \

        # Download and build nginx
        && wget --no-check-certificate http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
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
        --with-openssl=/root/download/openssl-$OPENSSL_VERSION \
        --with-ld-opt="-ljemalloc" \
        && make \
        && make install

CMD ["/opt/nginx/sbin/nginx","-g","daemon off;"]

EXPOSE 80 443


