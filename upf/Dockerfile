From golang:1.12.9
MAINTAINER SohailAnjum

ENV GO111MODULE=off
RUN echo 'export GOPATH=$HOME/go' >> ~/.bashrc
RUN echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
RUN echo 'export PATH=$PATH:$GOPATH/bin:$GOROOT/bin' >> ~/.bashrc
RUN echo 'export GO111MODULE=off' >> ~/.bashrc
RUN apt-get -y update
RUN apt-get -y install git gcc make cmake autoconf libtool pkg-config libmnl-dev libyaml-dev netcat tcpdump

# Build UPF
RUN cd /go/src \
    && git clone --recursive -b v3.0.2 -j `nproc` https://github.com/free5gc/free5gc.git \
    && cd free5gc \
    && git submodule update --init
RUN go get -u -v "github.com/sirupsen/logrus" 

RUN cd /go/src/free5gc/src/upf \
    && mkdir build \ 
    && cd build \
    && cmake .. \
    && make -j`nproc`

WORKDIR /go/src/free5gc/src/upf/build/bin

EXPOSE 29519


CMD ["/go/src/free5gc/src/upf/build/bin/free5gc-upfd"]

