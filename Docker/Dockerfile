# from docker hub
FROM centos:centos6

# install git
RUN yum install -y git htop wget curl libxml2-devel libxslt-devel tar patch gcc gcc-c++ yum-utils libffi-devel python-setuptools fontconfig

# install pyenv to root
RUN git clone https://github.com/yyuu/pyenv.git /root/.pyenv

RUN echo 'export PYENV_ROOT=$HOME/.pyenv' >> /root/.bashrc \
  && echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /root/.bashrc \
  && echo 'eval "$(pyenv init -)"' >> /root/.bashrc \
  && source ~/.bashrc

# install ansible
RUN easy_install pip \
  && yum install -y openssl-devel python-devel \
  && pip install 'requests[security]' \
  && pip install ansible

# install python 3.5.2
RUN /root/.pyenv/bin/pyenv install 3.5.2 \
  && /root/.pyenv/bin/pyenv global 3.5.2 \
  && /root/.pyenv/bin/pyenv rehash
RUN /root/.pyenv/shims/pip install --upgrade pip \
  && /root/.pyenv/shims/pip install wheel

# install mysql
RUN yum install -y http://repo.mysql.com/mysql-community-release-el6-7.noarch.rpm state=present \
  && yum-config-manager --disable mysql56-community \
  && yum-config-manager --enable mysql57-community \
  && yum install -y -q install mysql-community-common-5.7.15-1.el6.x86_64

# install Groonga
RUN yum install -y http://packages.groonga.org/centos/groonga-release-1.1.0-1.noarch.rpm
RUN yum install -y mysql57-community-mroonga mysql-devel

RUN rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql /var/run/mysqld \
	&& chown -R mysql:mysql /var/lib/mysql /var/run/mysqld \
# ensure that /var/run/mysqld (used for socket and lock files) is writable regardless of the UID our mysqld instance ends up having at runtime
	&& chmod 777 /var/run/mysqld
RUN mysqld --user=mysql --initialize-insecure --datadir="/var/lib/mysql"

RUN mkdir /tmp/phantomjs \
  && curl -L https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2 \
        | tar -xj --strip-components=1 -C /tmp/phantomjs \
  && mv /tmp/phantomjs/bin/phantomjs /usr/local/bin

RUN git clone https://github.com/n1k0/casperjs.git
RUN mv casperjs /opt/
RUN ln -sf /opt/casperjs/bin/casperjs /usr/local/bin/casperjs

ENTRYPOINT /etc/init.d/mysqld start && mysql -uroot -e "INSTALL PLUGIN mroonga SONAME 'ha_mroonga.so';" && /bin/bash
