# 拉取 CentOS
FROM daocloud.io/centos:latest

# 维护者
MAINTAINER leepin <admin@cxsir.com>

# 设置 PHP 版本
ENV PHP_VERSION 7.0.11

# 安装依赖
RUN set -x \
  && yum update -y \
  && yum install epel-release -y \
  && yum install -y wget git gcc gcc-c++ make automake autoconf perl tar re2c libjpeg libpng libjpeg-devel libpng-devel libjpeg-turbo freetype freetype-devel \
        libcurl-devel libxml2-devel libjpeg-turbo-devel libXpm-devel libXpm libicu-devel libmcrypt libmcrypt-devel libxslt-devel libxslt openssl openssl-devel bzip2-devel \
	
  # 建立目录
  && mkdir -p /opt/php \
	&& mkdir ~/download \
  && cd ~/download \

  # 编译 PHP
	&& wget http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
	&& tar -zxf php-$PHP_VERSION.tar.gz \
	&& cd php-$PHP_VERSION \
	&& ./configure \
       --prefix=/opt/php \
       --with-config-file-path=/opt/php/etc/ \
       --with-config-file-scan-dir=/opt/php/conf.d/ \
       --enable-fpm \
       --enable-cgi \
       --disable-phpdbg \
       --enable-mbstring \
       --enable-calendar \
       --with-xsl \
       --with-openssl \
       --enable-soap \
       --enable-zip \
       --enable-shmop \
       --enable-sockets \
       --with-gd \
       --with-freetype-dir=/usr/include/freetype2/freetype \
       --with-jpeg-dir \
       --with-png-dir \
       --with-xpm-dir \
       --with-xmlrpc \
       --enable-pcntl \
       --enable-intl \
       --with-mcrypt \
       --enable-sysvsem \
       --enable-sysvshm \
       --enable-sysvmsg \
       --enable-opcache \
       --with-iconv \
       --with-bz2 \
       --with-curl \
       --enable-mysqlnd \
       --with-mysqli=mysqlnd \
       --with-pdo-mysql=mysqlnd \
       --with-zlib \
       --with-gettext \
  && make \
	&& make install \

  # 复制配置文件
  && cp ~/download/php-$PHP_VERSION/php.ini-production /opt/php/etc/php.ini \
  && cp /opt/php/etc/php-fpm.conf.default /opt/php/etc/php-fpm.conf \
  && cp /opt/php/etc/php-fpm.d/www.conf.default /opt/php/etc/php-fpm.d/www.conf \

  # 安装 Redis 扩展
  && cd ~/download \
  && git clone -b php7 https://github.com/phpredis/phpredis.git \
  && cd phpredis \
  && /opt/php/bin/phpize \
  && ./configure --with-php-config=/opt/php/bin/php-config \
  && make \
  && make install \

  # 安装 Composer
  && cd ~/download \
  && /opt/php/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && /opt/php/bin/php composer-setup.php --install-dir=/usr/local/sbin --filename=composer \
  && /opt/php/bin/php -r "unlink('composer-setup.php');" \

  # 删除安装文件
  && rm -rf ~/download


CMD ["/opt/php/sbin/php-fpm"]

EXPOSE 9000
