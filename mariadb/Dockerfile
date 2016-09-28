# 使用 daocloud 的镜像国内速度快
FROM daocloud.io/alpine:latest

MAINTAINER Leepin.du <admin@cxsir.com>

# 设置 mariadb 版本
ENV MARIADB_VERSION 10.2.2
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
    && apk --update add --no-cache --virtual .build-deps git \
    	gzip \
    	gcc \
    	g++ \
    	make \
    	bison \
    	ncurses-dev \
    	zlib-dev \
    	cmake \
    	jemalloc-dev \
        openssl \
        linux-headers \
        openssl-dev \
        gnutls-dev \

    # 下载源码编译
    && mkdir ~/download \
    && mkdir /srv/mariadb \
    && mkdir /srv/database \
    && mkdir /srv/config \
    && cd ~/download \
    && wget https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-$MARIADB_VERSION/source/mariadb-$MARIADB_VERSION.tar.gz \
    && tar -zxvf mariadb-$MARIADB_VERSION.tar.gz \
    && cd mariadb-$MARIADB_VERSION \
    && cmake . \
    	-DCMAKE_INSTALL_PREFIX=/srv/mariadb \
    	-DMYSQL_DATADIR=/srv/database \
    	-DSYSCONFDIR=/srv/config \
    	-DWITH_INNOBASE_STORAGE_ENGINE=1 \
    	-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
    	-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
    	-DWITH_READLINE=1 \
    	-DWITH_SSL=system \
    	-DWITH_ZLIB=system \
    	-DWITH_LIBWRAP=0 \
    	-DMYSQL_UNIX_ADDR=/dev/shm/mysql.sock \
    	-DDEFAULT_CHARSET=utf8mb4 \
    	-DDEFAULT_COLLATION=utf8mb4_general_ci \

    && make \
    && make install \
    && rm -rf ~/download

