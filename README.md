# ubuntu-sshd
Star this repository if it is useful for you.  
[![Docker Stars](https://img.shields.io/docker/stars/takeyamajp/ubuntu-sshd.svg)](https://hub.docker.com/r/takeyamajp/ubuntu-sshd/)
[![Docker Pulls](https://img.shields.io/docker/pulls/takeyamajp/ubuntu-sshd.svg)](https://hub.docker.com/r/takeyamajp/ubuntu-sshd/)
[![license](https://img.shields.io/github/license/takeyamajp/docker-ubuntu-sshd.svg)](https://github.com/tiger7456/docker_dev/tree/master/LICENSE)

## Supported tags and respective Dockerfile links  
- [`latest`, `ubuntu20.04`](https://github.com/tiger7456/docker_dev/tree/master/ubuntu20.04/Dockerfile)
- [`ubuntu18.04`](https://github.com/tiger7456/docker_dev/tree/master/ubuntu18.04/Dockerfile)
- [`ubuntu16.04`](https://github.com/tiger7456/docker_dev/tree/master/ubuntu16.04/Dockerfile)

## Image summary
    FROM ubuntu:20.04  
    MAINTAINER "scanflove"
    
    ENV TZ Asia/Tokyo
    
    ENV ROOT_PASSWORD root
    
    EXPOSE 22

## How to use
This container can be accessed by SSH and SFTP clients.

    docker run -d --name ubuntu-sshd \  
           -e TZ=Asia/Shanghai \  
           -e ROOT_PASSWORD=root \  
           -p 8022:22 \  
           takeyamajp/ubuntu-sshd

You can add extra ports and volumes as follows if you want.

    docker run -d --name ubuntu-sshd \  
           -e TZ=Asia/Shanghai \  
           -e ROOT_PASSWORD=root \  
           -p 8022:22 \  
           -p 8080:80 \  
           -v /my/own/datadir:/var/www/html \  
           takeyamajp/ubuntu-sshd

SCP command can be used for transferring files.

    scp -P 8022 -r /my/own/apache2.conf root@localhost:/etc/apache2/apache2.conf

## Time zone
You can use any time zone such as America/Chicago that can be used in Ubuntu.  

See below for zones.  
https://www.unicode.org/cldr/charts/latest/verify/zones/en.html

## Logging
This container logs the beginning, authentication, and termination of each connection.  
Use the following command to view the logs in real time.

    docker logs -f ubuntu-sshd
