# from docker hub
FROM centos:centos6

MAINTAINER Urasin <urasin2012@gmail.com>

# install git
RUN yum install -y git htop wget curl libxml2-devel libxslt-devel tar patch gcc

# install mysql
RUN yum install -y http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
RUN yum update -y
RUN yum install -y mysql-community-server

# install pyenv to root
RUN git clone https://github.com/yyuu/pyenv.git /root/.pyenv

RUN echo 'export PYENV_ROOT=$HOME/.pyenv' >> /root/.bashrc
RUN echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /root/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> /root/.bashrc
RUN exec $SHELL

# install python 2.7.9
RUN yum install -y zlib-devel bzip2 bzip2-devel readline-devel sqlite-devel openssl-devel libjpeg-devel libselinux-python mysql-community-devel
RUN /root/.pyenv/bin/pyenv install 2.7.9

# install ansible
# ansibleのため/usr/bin/pythonにsymbolic linkをはる(ansibleはlocal, remoteの/ust/bin/pythonを見るため)
RUN mv /usr/bin/python /usr/bin/python_bak
RUN  ln -s /root/.pyenv/versions/2.7.9/bin/python /usr/bin/python
RUN /root/.pyenv/versions/2.7.9/bin/pip install ansible
RUN /root/.pyenv/bin/pyenv rehash

# install python 3.5.0
RUN /root/.pyenv/bin/pyenv install 3.5.0
RUN /root/.pyenv/bin/pyenv global 3.5.0

ENTRYPOINT /etc/init.d/mysqld start && /bin/bash

