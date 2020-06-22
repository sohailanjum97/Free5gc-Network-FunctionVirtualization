FROM golang:1.12.9

MAINTAINER SohailAnjum

ENV GO111MODULE=off
WORKDIR /go/src/free5gc

RUN echo 'export GOPATH=$HOME/go' >> ~/.bashrc
RUN echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
RUN echo 'export PATH=$PATH:$GOPATH/bin:$GOROOT/bin' >> ~/.bashrc
RUN echo 'export GO111MODULE=off' >> ~/.bashrc

RUN apt-get -y update 
RUN apt-get install git
RUN apt-get -y install gcc
RUN apt-get -y install cmake
RUN apt-get -y install pkg-config 
RUN apt-get -y install autoconf
RUN apt-get -y install libmnl-dev
RUN apt-get -y install libyaml-dev
RUN apt-get -y update
RUN apt-get -y install net-tools
RUN go get -u github.com/sirupsen/logrus

#RUN export GOPATH=/go/src 
RUN git clone --recursive -b v3.0.2 -j `nproc` https://github.com/free5gc/free5gc.git /opt/app
RUN mv /opt/app/* .

RUN ./install_env.sh
RUN ls -la 
RUN pwd
RUN go build -o bin/udr -x src/udr/udr.go
CMD ["./bin/udr"]
