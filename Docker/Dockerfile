# from docker hub
FROM centos:centos6

MAINTAINER Urasin <urasin2012@gmail.com>

# install git
RUN yum install -y git htop wget curl libxml2-devel libxslt-devel tar patch gcc gcc-c++ yum-utils libffi-devel

# install mysql
RUN yum install -y http://repo.mysql.com/mysql-community-release-el6-7.noarch.rpm state=present
RUN yum update -y
RUN yum-config-manager --disable mysql56-community
RUN yum-config-manager --enable mysql57-community

# install pyenv to root
RUN git clone https://github.com/yyuu/pyenv.git /root/.pyenv

RUN echo 'export PYENV_ROOT=$HOME/.pyenv' >> /root/.bashrc
RUN echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /root/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> /root/.bashrc
RUN source ~/.bashrc

# install python 2.7.9
RUN yum install -y zlib-devel bzip2 bzip2-devel readline-devel sqlite-devel openssl-devel libjpeg-devel libselinux-python
RUN /root/.pyenv/bin/pyenv install 2.7.9

# install ansible
RUN /root/.pyenv/versions/2.7.9/bin/pip install ansible
RUN /root/.pyenv/bin/pyenv rehash

# install python 3.5.2
RUN /root/.pyenv/bin/pyenv install 3.5.2
RUN /root/.pyenv/bin/pyenv global 3.5.2
RUN /root/.pyenv/bin/pyenv rehash
RUN /root/.pyenv/shims/pip install --upgrade pip
RUN /root/.pyenv/shims/pip install wheel

# install Groonga
RUN yum install -y http://packages.groonga.org/centos/groonga-release-1.1.0-1.noarch.rpm
RUN yum install -y mysql57-community-mroonga mysql-devel

# install redis
RUN yum -y install epel-release; yum clean all
RUN yum -y install redis; yum clean all

# ansibleのため/usr/bin/pythonにsymbolic linkをはる(ansibleはlocal, remoteの/ust/bin/pythonを見るため)
RUN mv /usr/bin/python /usr/bin/python_bak
RUN ln -s /root/.pyenv/versions/2.7.9/bin/python /usr/bin/python

RUN rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql /var/run/mysqld \
	&& chown -R mysql:mysql /var/lib/mysql /var/run/mysqld \
# ensure that /var/run/mysqld (used for socket and lock files) is writable regardless of the UID our mysqld instance ends up having at runtime
	&& chmod 777 /var/run/mysqld
RUN mysqld --user=mysql --initialize-insecure --datadir="/var/lib/mysql"
ENTRYPOINT echo "daemonize yes" > redis.conf && redis-server redis.conf && /etc/init.d/mysqld start && mysql -uroot -e "INSTALL PLUGIN mroonga SONAME 'ha_mroonga.so';" && /bin/bash
