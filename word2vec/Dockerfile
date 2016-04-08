FROM centos:centos7
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

RUN yum -y update && \
    yum -y install gcc gcc-c++ gcc-gfortran automake libtool && \
    yum -y install git wget curl tar openssl openssl-devel && \
    yum -y install epel-release && \
    yum -y install python34 python34-devel blas-devel lapack-devel && \
    yum clean all && \
    mkdir -p /opt/tools

WORKDIR /opt/tools
RUN git clone https://github.com/pinmarch/word2vec.git && \
    cd word2vec && make

WORKDIR /opt/tools
RUN wget https://bootstrap.pypa.io/get-pip.py && \
    python3.4 get-pip.py

RUN pip3 install numpy && pip3 install scipy
RUN pip3 install pandas

WORKDIR /opt/tools
RUN wget https://mecab.googlecode.com/files/mecab-0.996.tar.gz && \
    tar xvfz mecab-0.996.tar.gz && \
    cd mecab-0.996 && \
    ./configure --with-charset=utf8 && make && make install

WORKDIR /opt/tools
RUN wget https://mecab.googlecode.com/files/mecab-ipadic-2.7.0-20070801.tar.gz && \
    tar xvfz mecab-ipadic-2.7.0-20070801.tar.gz && \
    cd mecab-ipadic-2.7.0-20070801 && \
    ./configure --with-charset=utf8 && make && make install

RUN pip3 install mecab-python3 && LANG=en_US.UTF-8 pip3 install gensim

RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG ja_JP.utf8
ENV LD_LIBRARY_PATH /usr/local/lib:$LD_LIBRARY_PATH

RUN for f in $(find /opt/tools/word2vec/* -executable | grep -v '.sh'); \
    do mv $f /usr/local/bin/ ; \
    done && \
    rm -rf /opt/tools
WORKDIR /

VOLUME /var/data/word2vec

CMD /bin/bash

