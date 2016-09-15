# from docker hub
FROM centos:centos6

MAINTAINER Urasin <urasin2012@gmail.com>

# install git
RUN yum install -y git htop wget curl libxml2-devel libxslt-devel tar patch gcc gcc-c++

# install mysql
RUN yum install -y http://repo.mysql.com/mysql-community-release-el6-7.noarch.rpm state=present
RUN yum update -y
RUN yum install -y -q install mysql-community-server-5.6.29-2.el6.x86_64
RUN yum install -y -q install mysql-community-devel-5.6.29-2.el6.x86_64

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
RUN yum install -y mysql-community-mroonga

# ansibleのため/usr/bin/pythonにsymbolic linkをはる(ansibleはlocal, remoteの/ust/bin/pythonを見るため)
RUN mv /usr/bin/python /usr/bin/python_bak
RUN ln -s /root/.pyenv/versions/2.7.9/bin/python /usr/bin/python

ENTRYPOINT /etc/init.d/mysqld start && /bin/bash
