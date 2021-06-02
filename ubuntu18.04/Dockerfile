FROM ubuntu:18.04
LABEL maintainer="scanflove"

# timezone
RUN apt update && apt install -y tzdata; \
    apt clean;


# base
RUN apt-get install -y build-essential vim curl gdb autoconf libtool unzip net-tools
RUN apt-get install -y wget
# RUN apt-get install -y git
RUN apt-get install -y gcc apt-utils
RUN apt-get install -y g++ cmake


# golang
RUN wget -P /tmp https://studygolang.com/dl/golang/go1.16.4.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf /tmp/go1.16.4.linux-amd64.tar.gz
RUN rm /tmp/go1.16.4.linux-amd64.tar.gz

ENV GOPATH /usr/local/go
ENV PATH $GOPATH/bin:/usr/local/go/bin:/root/go/bin:$PATH
RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
# 启用 Go Modules 功能
ENV GO111MODULE on
# 配置 GOPROXY 环境变量
ENV GOPROXY https://goproxy.cn

RUN echo 'export GOROOT=/usr/local/go\n\
export PATH=$PATH:$GOROOT/bin:/root/go/bin\n\
export GO111MODULE=on\n\
export GOPROXY=https://goproxy.cn\n'\
>/etc/profile

# gRPC环境安装
RUN apt install -y golang-goprotobuf-dev
RUN go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway
RUN go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger
RUN go get -u github.com/golang/protobuf/protoc-gen-go
RUN go get -u google.golang.org/grpc/cmd/protoc-gen-go-grpc
# delve调试环境搭建
RUN go get -u github.com/go-delve/delve/cmd/dlv


# dos2unix 实际上它安装了两个工具：todos（相当于unix2dos），和fromdos（相当于dos2unix）
RUN apt-get install -y tofrodos 
RUN ln -s /usr/bin/todos /usr/bin/unix2dos 
RUN ln -s /usr/bin/fromdos /usr/bin/dos2unix 

# 编译环境
RUN ln -s /usr/share/aclocal /usr/local/share/aclocal

# JDK
# RUN apt-get install -y openjdk-8-jdk
# 执行命令(这里创建了一个目录)
# RUN mkdir /usr/local/java
# 和copy一样，复制文件到指定目录，但是copy不能解压，add自动解压
# ADD jdk-8u181-linux-x64.tar.gz /usr/local/java
# 重命名(不知道文件名可以现在宿主机解压后看一下)
# RUN ln -s /usr/local/java/jdk1.8.0_181 /usr/local/java/jdk 
# 设置环境变量 
# ENV JAVA_HOME /usr/local/java/jdk 
# ENV JRE_HOME ${JAVA_HOME}/jre 
# ENV CLASSPATH .:${JAVA_HOME}/lib:${JRE_HOME}/lib 
# ENV PATH ${JAVA_HOME}/bin:$PATH 

# sshd
RUN mkdir /run/sshd; \
    apt install -y openssh-server; \
    sed -i 's/^#\(PermitRootLogin\) .*/\1 yes/' /etc/ssh/sshd_config; \
    sed -i 's/^\(UsePAM yes\)/# \1/' /etc/ssh/sshd_config; \
    apt clean;

# entrypoint
RUN { \
    echo '#!/bin/bash -eu'; \
    echo 'ln -fs /usr/share/zoneinfo/${TZ} /etc/localtime'; \
    echo 'echo "root:${ROOT_PASSWORD}" | chpasswd'; \
    echo 'exec "$@"'; \
    } > /usr/local/bin/entry_point.sh; \
    chmod +x /usr/local/bin/entry_point.sh;

ENV TZ Asia/Tokyo

ENV ROOT_PASSWORD root

EXPOSE 22

ENTRYPOINT ["entry_point.sh"]
CMD    ["/usr/sbin/sshd", "-D", "-e"]
