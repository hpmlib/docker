# 拉取 CentOS
FROM daocloud.io/centos:latest

# 维护者
MAINTAINER leepin <admin@cxsir.com>

# 设置版本
ENV MEMCACHED_VERSION 1.4.13

# 安装依赖
RUN set -x \
	&& yum install wget libevent-devel -y \
	&& wget http://www.memcached.org/files/memcached-$MEMCACHED_VERSION.tar.gz \
	&& cd memcached-$MEMCACHED_VERSION \
	&& ./configure --prefix=/opt/memcached \
	&& make \
	&& make install \
	&& rm -rf memcached-$MEMCACHED_VERSION.tar.gz

CMD ['memcached']

EXPOSE 11211


