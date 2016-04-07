FROM b.gcr.io/tensorflow/tensorflow:latest-gpu
MAINTAINER Mike Orzel <mike.orzel@emergingstack.com>

# Add some dependent packages we will need for the build process
RUN apt-get -y update && \
    apt-get -y install git bc make dpkg-dev libssl-dev pkg-config pciutils

RUN apt-get -y install software-properties-common python-software-properties


# Run jupyter notebook and create a folder for the notebooks
RUN chmod +x /run_jupyter.sh
RUN mkdir /examples
WORKDIR /examples
ADD CNN.ipynb /examples/
CMD /run_jupyter.sh

