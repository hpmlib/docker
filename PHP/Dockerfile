# Pull Base image
FROM daocloud.io/centos:latest

# MAINTAINER
MAINTAINER leepin <admin@cxsir.com>

# set php version
ENV PHP_VERSION=7.0.10

RUN set -x \
  && yum update -y \
  && yum install epel-release -y \
  && yum install -y wget git gcc gcc-c++ make automake autoconf perl tar re2c libjpeg libpng libjpeg-devel libpng-devel libjpeg-turbo freetype freetype-devel \
        libcurl-devel libxml2-devel libjpeg-turbo-devel libXpm-devel libXpm libicu-devel libmcrypt libmcrypt-devel libxslt-devel libxslt openssl openssl-devel bzip2-devel \
	&& mkdir -p /opt/php \

# Download PHP and build php
	&& cd ~ \
	&& wget http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
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
  && cp ~/php-$PHP_VERSION/php.ini-production /opt/php/etc/php.ini \
  && cp /opt/php/etc/php-fpm.conf.default /opt/php/etc/php-fpm.conf \
  && cp /opt/php/etc/php-fpm.d/www.conf.default /opt/php/etc/php-fpm.d/www.conf

RUN set -x \
  # install php extension
  && cd ~ \
  && git clone -b php7 https://github.com/phpredis/phpredis.git \
  && cd phpredis \
  && /opt/php/bin/phpize \
  && ./configure --with-php-config=/opt/php/bin/php-config \
  && make \
  && make install

CMD ["/opt/php/sbin/php-fpm"]

EXPOSE 9000