# 使用 daocloud 的镜像国内速度快
FROM daocloud.io/alpine:latest

MAINTAINER Leepin.du <admin@cxsir.com>

# 设置 nginx openssl timezone
ENV PHP_VERSION 7.0.11
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
		    libjpeg \
		    libpng-dev \
		    freetype-dev \
		    libcurl \
		    libmcrypt-dev \
		    libxml2-dev \
		    libxpm-dev \
		    libxslt-dev \
		    git \
    
    # 下载并编译
	&& mkdir ~/download \
	&& cd ~/download \
	&& wget --no-check-certificate http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
	&& tar -zxf php-$PHP_VERSION.tar.gz \
	&& cd php-$PHP_VERSION \
	&& './configure' \
	   '--prefix=/opt/php' \
	   '--with-config-file-path=/opt/php/etc/' \
	   '--with-config-file-scan-dir=/opt/php/conf.d/' \
	   '--enable-fpm' \
	   '--enable-cgi' \
	   '--disable-phpdbg' \
	   '--enable-mbstring' \
	   '--enable-calendar' \
	   '--with-xsl' \
	   '--with-openssl' \
	   '--enable-soap' \
	   '--enable-zip' \
	   '--enable-shmop' \
	   '--enable-sockets' \
	   '--with-gd' \
	   '--with-freetype-dir=/usr/include/freetype2/freetype' \
	   '--with-jpeg-dir' \
	   '--with-png-dir' \
	   '--with-xpm-dir' \
	   '--with-xmlrpc' \
	   '--with-pear' \
	   '--enable-pcntl' \
	   '--enable-intl' \
	   '--with-mcrypt' \
	   '--enable-sysvsem' \
	   '--enable-sysvshm' \
	   '--enable-sysvmsg' \
	   '--enable-opcache' \
	   '--with-iconv' \
	   '--with-bz2' \
	   '--with-curl' \
	   '--enable-mysqlnd' \
	   '--with-mysqli=mysqlnd' \
	   '--with-pdo-mysql=mysqlnd' \
	   '--with-zlib' \
	   '--with-gettext' \
	   '--disable-fileinfo' \
	&& make \
	&& make install \
	&& cp ~/download/php-$PHP_VERSION/php.ini-production /opt/php/etc/php.ini \
	&& cp /opt/php/etc/php-fpm.conf.default /opt/php/etc/php-fpm.conf \
	&& cp /opt/php/etc/php-fpm.d/www.conf.default /opt/php/etc/php-fpm.d/www.conf \
	
	# install php extension
	&& cd ~/download \
	&& git clone -b php7 https://github.com/phpredis/phpredis.git \
	&& cd phpredis \
	&& /opt/php/bin/phpize \
	&& ./configure --with-php-config=/opt/php/bin/php-config \
	&& make \
	&& make install \
	&& rm -rf ~/download

CMD ["/opt/php/sbin/php-fpm"]

EXPOSE 9000
