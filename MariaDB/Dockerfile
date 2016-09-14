# Pull Base image
FROM daocloud.io/centos:latest

# MAINTAINER
MAINTAINER leepin <admin@cxsir.com>

RUN set -x \
  # 这里不知道咋回事 用 cat eof 和 tee eof 都写不进去
  && echo "[mariadb]" > /etc/yum.repos.d/MariaDB.repo \
  && echo "name = MariaDB" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "baseurl=http://yum.mariadb.org/10.1/centos7-amd64" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" >> /etc/yum.repos.d/MariaDB.repo \
  && echo "gpgcheck=1" >> /etc/yum.repos.d/MariaDB.repo \
  && yum clean all \
  && yum update -y \
  && yum install MariaDB-server MariaDB-client -y

CMD ["/etc/init.d/mysql","start"]
EXPOSE 3306
