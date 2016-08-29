# Pull Base image
FROM daocloud.io/centos:latest

# MAINTAINER
MAINTAINER leepin <admin@cxsir.com>

RUN set -x \
  && echo "[mariadb]" > /etc/yum.repos.d/MariaDB.repo \
  && echo "name = MariaDB" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "baseurl=http://yum.mariadb.org/10.1/centos7-amd64" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "gpgcheck=1" >> /etc/yum.repos.d/MariaDB.repo \
  && yum clean all \
  && yum update -y \
  && yum install MariaDB-server MariaDB-client -y

ADD my.cnf /etc/mysql/my.cnf

CMD ["/etc/init.d/mysqld","start"]
EXPOSE 3306
