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
RUN  ln -s /root/.pyenv/versions/2.7.9/bin/pip /usr/bin/python
RUN /root/.pyenv/versions/2.7.9/bin/pip install ansible

# pyenv global(python 3.4.3)
RUN /root/.pyenv/bin/pyenv global system
