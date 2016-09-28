# 使用 daocloud 的镜像国内速度快
FROM daocloud.io/alpine:latest

MAINTAINER Leepin.du <admin@cxsir.com>

# 设置 redis 版本
ENV REDIS_VERSION 3.2.24
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
    && apk --update add wget \
    	gcc \
    	g++ \
    	make \
    	perl \

    # 编译 redis
	&& cd /srv \
	&& wget http://download.redis.io/releases/redis-$REDIS_VERSION.tar.gz \
	&& tar -zxvf redis-$REDIS_VERSION.tar.gz \
	&& rm -f redis-$REDIS_VERSION.tar.gz \
	&& mv redis-$REDIS_VERSION redis \
	&& cd redis \
	&& make 

CMD ["/srv/redis/src/redis-server","/app/redis.conf"]

EXPOSE 6379
