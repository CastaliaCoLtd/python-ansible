# from docker hub
FROM hardbyte/python3
MAINTAINER Urasin <urasin2012@gmail.com>

# install pyenv to root
RUN git clone https://github.com/yyuu/pyenv.git /root/.pyenv

RUN echo 'export PYENV_ROOT=$HOME/.pyenv' >> /root/.bashrc
RUN echo 'export PATH=$PYENV_ROOT/bin:$PATH' >> /root/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> /root/.bashrc
RUN exec $SHELL

# install python 2.7.9
RUN /root/.pyenv/bin/pyenv install 2.7.9

# install ansible
# ansibleのため/usr/bin/pythonにsymbolic linkをはる(ansibleはlocal, remoteの/ust/bin/pythonを見るため)
RUN  ln -s /root/.pyenv/versions/2.7.9/bin/python /usr/bin/python
RUN /root/.pyenv/versions/2.7.9/bin/pip install ansible

# pyenv global(python 3.4.3)
RUN /root/.pyenv/bin/pyenv global system

# for pyquery
RUN apt-get -y install libxslt1-dev libxml2-dev

# for mysql
ADD http://dev.mysql.com/get/mysql-apt-config_0.1.5-2ubuntu14.04_all.deb /tmp/
RUN DEBIAN_FRONTEND=noninteractive dpkg -i /tmp/mysql-apt-config_0.1.5-2ubuntu14.04_all.deb
RUN DEBIAN_FRONTEND=noninteractive apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install mysql-server

ENTRYPOINT /etc/init.d/mysql start && /bin/bash

