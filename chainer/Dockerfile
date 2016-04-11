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

RUN pip install -U setuptools cython pillow protobuf h5py


WORKDIR /opt

# download chainer samples
RUN git clone https://github.com/pfnet/chainer.git

ENV drivertag NVIDIA-Linux-x86_64-361.28
ADD ${drivertag}.run /opt/
RUN chmod +x ${drivertag}.run && \
    ./${drivertag}.run -x && \
    ln -s $(basename $(ls -1 ${drivertag}/libcuda.so.* | head -n1)) ${drivertag}/libcuda.so && \
    echo /opt/${drivertag} >> /etc/ld.so.conf.d/nvidia.conf && \
    ldconfig

RUN ln -s /opt/${drivertag}/nvidia-smi /usr/local/bin/


# install chainer (as libcuda.so is required, execute here)
ENV CUDA_ROOT /usr/local/cuda
ENV CUDA_INC_DIR $CUDA_ROOT/bin:$CUDA_INC_DIR
ENV CPATH $CPATH:$CUDA_ROOT/include:/usr/include
ENV LD_LIBRARY_PATH $CUDA_ROOT/lib:$CUDA_ROOT/lib64:/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

LABEL chainer.version="1.7.2"
RUN LIBRARY_PATH=/opt/${drivertag} pip install chainer

RUN apt-get -y install python-pandas python-matplotlib ipython ipython-notebook && \
    apt-get autoclean

VOLUME /var/data/chainer
WORKDIR /root

CMD /bin/bash

