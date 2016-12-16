# 使用 daocloud 的镜像国内速度快
FROM daocloud.io/alpine:latest

MAINTAINER leongpeng <admin@cxsir.com>

# 设置 nginx openssl timezone
ENV OPENSSL_VERSION 1.0.2h
ENV NGINX_VERSION 1.10.2
ENV TIMEZONE Asia/Shanghai

# 设置为中国源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
    && apk update \
    && apk upgrade \

    # 设置时区
    && apk add tzdata \
    && ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
    && echo $TIMEZONE > /etc/timezone \

    # 安装依赖
    && apk --update add --no-cache wget \
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
		perl \

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
    && make install \
    && rm -rf ~/download

CMD ["/opt/nginx/sbin/nginx","-g","daemon off;"]

EXPOSE 80 443


