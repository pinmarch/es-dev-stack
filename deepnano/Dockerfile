FROM stephenturner/poretools:latest
MAINTAINER pinmarch <pinmarch.at.sakura@gmail.com>

LABEL deepnano.version="20160409"

RUN pip install -U cython numpy h5py Theano python-dateutil watchdog

RUN mkdir -p /opt
WORKDIR /opt
RUN git clone https://bitbucket.org/vboza/deepnano.git

RUN cd deepnano ; g++ -O2 -std=gnu++0x align_2d.cc -o align_2d
RUN echo '#!/bin/sh\n\ncd /opt/deepnano\npython basecall.py $@' \
      >/usr/local/bin/basecall && \
    chmod +x /usr/local/bin/basecall && \
    echo '#!/bin/sh\n\ncd /opt/deepnano\npython basecall_no_metrichor.py $@' \
      >/usr/local/bin/basecall_no_metrichor && \
    chmod +x /usr/local/bin/basecall_no_metrichor


VOLUME /var/data/deepnano
WORKDIR /root

ENTRYPOINT /bin/bash

