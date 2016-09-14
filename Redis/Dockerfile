# Pull Base image
FROM daocloud.io/centos:latest

# MAINTAINER
MAINTAINER leepin <admin@cxsir.com>

# REDIS VERSION
ENV REDIS_VERSION=3.2.3

# Download and Build it.
RUN set -x \
  && yum update -y \
  && yum install -y wget gcc gcc-c++ make perl tar \
  && cd /opt \
  && wget http://download.redis.io/releases/redis-$REDIS_VERSION.tar.gz \
  && tar -zxvf redis-$REDIS_VERSION.tar.gz \
  && rm -f redis-$REDIS_VERSION.tar.gz \
  && mv redis-$REDIS_VERSION redis \
  && cd redis \
  && make

CMD ["/opt/redis/src/redis-server","/app/redis.conf"]

EXPOSE 6379
