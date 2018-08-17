# from docker hub
FROM centos:centos7.1.1503

MAINTAINER Castalia Developers <developers@castalia.co.jp>


# install git
RUN yum install -y git wget curl libxml2-devel libxslt-devel tar patch gcc gcc-c++ yum-utils libffi-devel

RUN rpm --rebuilddb;yum install -y yum-plugin-ovl
RUN yum swap -y fakesystemd systemd

RUN yum update -y && yum -y install glibc-common && localedef -v -c -i ja_JP -f UTF-8 ja_JP.UTF-8; echo "";
ENV LANG=ja_JP.UTF-8

# install mysql	client
RUN rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm && yum -y install mysql-community-client

# install pyenv to root
RUN git clone https://github.com/yyuu/pyenv.git /root/.pyenv

RUN echo 'export PYENV_ROOT=$HOME/.pyenv' >> /root/.bashrc
RUN echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /root/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> /root/.bashrc
RUN source ~/.bashrc

# install python 3.6.1
RUN yum install -y make zlib-devel bzip2 bzip2-devel readline-devel sqlite-devel openssl-devel libjpeg-devel libselinux-python

RUN /root/.pyenv/bin/pyenv install -v 3.6.1
RUN /root/.pyenv/bin/pyenv global 3.6.1
RUN /root/.pyenv/versions/3.6.1/bin/pip install ansible
RUN /root/.pyenv/bin/pyenv rehash
RUN /root/.pyenv/shims/pip install --upgrade pip
RUN /root/.pyenv/shims/pip install wheel

ENTRYPOINT /bin/bash
