FROM ubuntu:18.04
LABEL maintainer="scanflove"

# timezone
RUN apt update && apt install -y tzdata; \
    apt clean;


# base
RUN apt-get install -y wget git gcc apt-utils  g++  cmake build-essential vim curl gdb 

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
