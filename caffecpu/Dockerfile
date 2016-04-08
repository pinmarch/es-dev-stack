FROM ubuntu:latest
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

RUN apt-get -y update && \
    apt-get -y install libatlas-base-dev libprotobuf-dev libleveldb-dev && \
    apt-get -y install libsnappy-dev libopencv-dev libhdf5-serial-dev && \
    apt-get -y install libgflags-dev libgoogle-glog-dev liblmdb-dev && \
    apt-get -y install --no-install-recommends libboost-all-dev && \
    apt-get autoclean
RUN apt-get -y install git python-opencv protobuf-compiler && \
    apt-get -y install python-pip python-numpy python-skimage && \
    apt-get autoclean

RUN git clone https://github.com/BVLC/caffe.git
WORKDIR caffe
RUN cp Makefile.config.example Makefile.config && \
    sed -i -e "s/^# CPU_ONLY/CPU_ONLY/" Makefile.config && \
    make all && make test && make runtest

RUN for f in $(cat python/requirements.txt); do pip install $f ; done
RUN make pycaffe

ENV PYTHONPATH /caffe/python:$PYTHONPATH

CMD /bin/bash


