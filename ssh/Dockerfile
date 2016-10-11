FROM ubuntu:latest

MAINTAINER Marcell Mars "https://github.com/marcellmars"

#---- this is convenient setup with cache for local development ---------------
# RUN echo 'Acquire::http::Proxy "http://172.17.42.1:3142";' >> /etc/apt/apt.conf.d/01proxy

RUN locale-gen en_US en_US.UTF-8


RUN apt-get update \
    && mkdir -p /var/run/sshd \
    && apt-get -y install openssh-server \
                          python-pip \
                          dnsmasq-base \
    && pip install supervisor \
                   supervisor-stdout \
                   tailer \
    && echo conf-dir=/etc/dnsmasq.d >> /etc/dnsmasq.conf \
    && echo user=root >> /etc/dnsmasq.conf

RUN useradd tunnel \
    && passwd -d tunnel

ADD supervisord.conf /etc/
ADD dnsmasq.conf /etc/supervisor/conf.d/
ADD dnsmasq.local /etc/dnsmasq.d/local
  
ADD sshd_config_tunnel /etc/ssh/
ADD ssh_tunnel.conf /etc/supervisor/conf.d/
ADD socket_server.conf /etc/supervisor/conf.d/

ADD socket_server.py /usr/local/bin/
RUN chmod +x /usr/local/bin/socket_server.py

ADD print_log.py /usr/local/bin/
RUN chmod +x /usr/local/bin/print_log.py