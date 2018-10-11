# Dockerfile
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y gcc g++ make cmake git-core libevent-dev libev-dev libssl-dev libffi-dev psmisc iptables zip unzip python-dev python-pip vim curl
# build libcouchbase
RUN git clone --branch 2.9.4 git://github.com/couchbase/libcouchbase.git && \
    mkdir libcouchbase/build
WORKDIR libcouchbase/build
RUN ../cmake/configure --prefix=/usr && \
      make && \
      make install
WORKDIR /
RUN git clone --branch 5.0 https://github.com/couchbaselabs/try-cb-python.git
WORKDIR try-cb-python
COPY 0001-Kubernetes-compatible.patch 0001-Kubernetes-compatible.patch
RUN git apply 0001-Kubernetes-compatible.patch
# install python deps
RUN pip2 install --upgrade packaging appdirs
RUN pip install -U pip setuptools
RUN pip install paramiko &&\
    pip install gevent &&\
    pip install boto &&\
    pip install httplib2 &&\
    pip install pyyaml &&\
    pip install kubernetes &&\
    pip install couchbase
RUN pip install -r requirements.txt
COPY entrypoint.sh entrypoint.sh
RUN chmod +x ./entrypoint.sh
ENTRYPOINT ["sh", "entrypoint.sh"]
