FROM b.gcr.io/tensorflow/tensorflow:latest-gpu
#MAINTAINER Mike Orzel <mike.orzel@emergingstack.com>
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

# Add some dependent packages we will need for the build process
RUN apt-get -y update && \
    apt-get -y install git bc make dpkg-dev libssl-dev pkg-config pciutils

RUN apt-get -y install software-properties-common python-software-properties && \
    apt-get autoclean && \
    mkdir -p /opt

# in this case, libcudnn.so does not exist, so create it.
RUN ( \
     libcudnn=$(find / -name libcudnn.so.* | head -n1) ; \
     ln -s $libcudnn $(dirname $libcudnn)/libcudnn.so \
    )

WORKDIR /opt
ENV drivertag NVIDIA-Linux-x86_64-361.28
ADD ${drivertag}.run /opt/
RUN chmod +x ${drivertag}.run && \
    ./${drivertag}.run -x && \
    echo /opt/${drivertag} >> /etc/ld.so.conf.d/nvidia.conf && \
    ldconfig && \
    ln -s /opt/${drivertag}/nvidia-smi /usr/local/bin/

WORKDIR /notebooks

# Run jupyter notebook and create a folder for the notebooks
RUN chmod +x /run_jupyter.sh
ADD CNN.ipynb /notebooks/

VOLUME /notebooks

EXPOSE 8888 6006

CMD /run_jupyter.sh

