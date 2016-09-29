# 拉取 CentOS
FROM daocloud.io/centos:latest

# 维护者
MAINTAINER leepin <admin@cxsir.com>

# 安装 beanstalkd
RUN set -x \
	&& yum update -y \
	&& yum upgrade -y \
	&& yum install epel-release -y \
	&& yum install beanstalkd -y

CMD ['beanstalkd', '-b', '/app']

EXPOSE 11300
