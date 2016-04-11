FROM ubuntu:latest
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

RUN apt-get -y update && \
    apt-get -y install libatlas-base-dev libprotobuf-dev libleveldb-dev && \
    apt-get -y install libsnappy-dev libopencv-dev libhdf5-serial-dev && \
    apt-get -y install libgflags-dev libgoogle-glog-dev liblmdb-dev && \
    apt-get -y install --no-install-recommends libboost-all-dev && \
    apt-get autoclean
RUN apt-get -y install git python-opencv protobuf-compiler && \
    apt-get -y install python-pip python-numpy python-scipy python-skimage && \
    apt-get -y install python-pandas python-matplotlib ipython ipython-notebook && \
    apt-get autoclean
RUN apt-get -y install openjdk-7-jdk r-base r-base-dev r-cran-rodbc && \
    apt-get autoclean


RUN git clone https://github.com/apache/spark.git 
WORKDIR /spark
ARG JAVA_TOOL_OPTIONS
RUN git pull && git checkout branch-1.6 && \
    build/mvn -Pyarn -Phadoop-2.6 -Psparkr -Phive -Phive-thriftserver \
              -Dhadoop.version=2.7.2 -DskipTests clean package

ENV PATH $PATH:/spark/bin:/spark/sbin

VOLUME /root
WORKDIR /root

CMD /bin/bash

