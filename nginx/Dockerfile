FROM daocloud.io/alpine:latest

MAINTAINER Leepin.du <admin@cxsir.com>

ENV OPENSSL_VERSION 1.0.2h
ENV NGINX_VERSION 1.10.1

RUN apk --update add wget \
		gcc gcc-c++ \
		make automake autoconf \
		libtool perl tar \
		bzip2-devel bzip2 \
		openssl openssl-devel \
		zlib zlib-devel \

	# Download openssl
	&& mkdir ~/download \
	&& cd ~/download \
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
	--with-openssl=/root/download/openssl-$OPENSSL_VERSION \
	&& make \
	&& make install 

CMD ["/opt/nginx/sbin/nginx","-g","daemon off;"]

EXPOSE 80 443


