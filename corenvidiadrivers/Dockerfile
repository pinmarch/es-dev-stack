FROM ubuntu:14.04
# MAINTAINER Mike Orzel <mike.orzel@emergingstack.com>
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

RUN apt-get -y update && \
    apt-get -y install git bc make dpkg-dev libssl-dev pkg-config xserver-xorg pciutils && \
    apt-get -y install software-properties-common python-software-properties && \
    apt-get autoclean && \
    mkdir -p /usr/src/kernels && \
    mkdir -p /opt/nvidia/nvidia_installers

# install gcc 4.9 for newer kernels
RUN add-apt-repository ppa:ubuntu-toolchain-r/test
RUN apt-get update && \
    apt-get install -y gcc-4.9 g++-4.9 && \
    apt-get autoclean && \
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-4.9

WORKDIR /usr/src/kernels
RUN git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux
WORKDIR linux
RUN git checkout -b stable v`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"` && \
    zcat /proc/config.gz > .config && make modules_prepare
RUN sed -i -e "s/`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"`/`uname -r`/" \
    include/generated/utsrelease.h # In case a '+' was added


#ADD http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run /opt/nvidia/
#ADD cuda_7.5.18_linux.run /opt/nvidia/
ADD NVIDIA-Linux-x86_64-361.28.run /opt/nvidia/nvidia_installers/
ADD mkdevs.sh /opt/nvidia/nvidia_installers/

# Nvidia drivers setup
WORKDIR /opt/nvidia/nvidia_installers

RUN chmod +x ./mkdevs.sh && \
    chmod +x ./NVIDIA-Linux-x86_64-361.28.run && \
    ./NVIDIA-Linux-x86_64-361.28.run -a -x --ui=none
CMD ./NVIDIA-Linux-x86_64-361.28/nvidia-installer -q -a -n -s \
      --kernel-source-path=/usr/src/kernels/linux/ && \
    modprobe nvidia_uvm && \
    ./mkdevs.sh

