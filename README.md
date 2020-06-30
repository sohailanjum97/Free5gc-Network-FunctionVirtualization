# Free5gc-Network-FunctionVirtualization
In this Project we used free5gc network function and make a dockerfile for each network function and run via docker-compose file. Its a complete project with configuration
files and folders just clone it and run the docker-compose file

This is the Lab 3 where we can make Dockerfiles and build the images.

**1. Environment Setting**

Linux 18.04

**2. Kernel version Setting**

**2.1 View the currently used kernel version**

    uname -a
    lsb_release -a

**2.2 View the currently available kernel version**

    apt-cache search linux-image
    dpkg --get-selections |grep linux
    dpkg --list |grep linux-image

**2.3 Install a favorite kernel version**

    sudo apt install linux-image-5.0.0-23-generic linux-headers-5.0.0-23 linux-modules-5.0.0-23-generic

**2.4 Uninstall the unwanted kernel version**

    sudo apt purge linux-image-5.3.0-46-generic linux-headers-5.3.0-46 linux-modules-5.3.0-46-generic
    sudo apt autoremove
    
**3. Docker Installation**
        
    sudo apt-get update
    sudo apt-get install docker.io
    
**4. Installtion of Docker-compose**
    
    sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    docker-compose --version

**5. Dockerfile Preparation**

    In order to make dockerfile we used free5gc stage 3 installtion instruction see the sample dockerfiles for different network functions in the folders. Here is the link
    of free4gc stage3 for refernce. [https://github.com/free5gc/free5gc]

**6. Dockerfile Preparation steps**
all the given commands related to run network functions such as we checked the dependencies from free5gc stage 3.

    
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


    RUN git clone --recursive -b v3.0.2 -j `nproc` https://github.com/free5gc/free5gc.git /opt/app
    RUN mv /opt/app/* .

    RUN ./install_env.sh
    RUN ls -la 
    RUN pwd
    RUN go build -o bin/amf -x src/amf/amf.go
    CMD ["./bin/amf"]

    EXPOSE 29511


**7. Build the Dockerfile**

    docker build t <image_name> .

    docker run rm it <image_name> /bin/bash

    docker exec it <container_name> /bin/bash
    
    docker ps -a
    
    docker inspect <container_id>
    
**8. Docker Composefile**

docker-compose is a tool which is used to run multi-container microservices using docker-compose file.

**8.1 start all Docker Containers**
    
    docker-compose up -d

**8.2 execution status of Docker Container**
 
    docker-compose ps
    
**8.3 see the execution log**

    docker-compose logs
    
**8.4 want to stop all containers**

     docker-compose stop
  
    
