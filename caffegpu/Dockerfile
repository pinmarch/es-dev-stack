FROM nvidia/cuda:7.5-cudnn4-devel-ubuntu14.04
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

LABEL caffe.commit="d21772c"

RUN mkdir -p /opt && cd /opt && \
    git clone https://github.com/BVLC/caffe.git
WORKDIR /opt/caffe
RUN cp Makefile.config.example Makefile.config && \
    sed -i -e "s/# USECUDNN/USECUDNN/" Makefile.config && \
    make all -j4

RUN for f in $(cat python/requirements.txt); do pip install $f ; done
RUN make pycaffe

WORKDIR /opt
ENV drivertag NVIDIA-Linux-x86_64-361.28
ADD ${drivertag}.run /opt/
RUN chmod +x ${drivertag}.run && \
    ./${drivertag}.run -x && \
    echo /opt/${drivertag} >> /etc/ld.so.conf.d/nvidia.conf && \
    ldconfig

WORKDIR /

ENV PYTHONPATH /opt/caffe/python:$PYTHONPATH

CMD /bin/bash

