FROM ubuntu:20.04
MAINTAINER Stanislav Slow <stanislav.rimsha@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ARG SSH_PORT
ENV SSH_PORT=${SSH_PORT}

RUN sed -i -e 's/http:\/\/archive/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list && \
    sed -i -e 's/http:\/\/security/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list

RUN apt-get update  && \
    apt-get install -y \
    sudo \
    git \
    mc \
    supervisor \
    openssh-server \
    curl \
    cron \
    wget \
    nano \
    htop \
    unzip \
    language-pack-ru && \
    apt-get autoremove -y && \
    apt-get clean && \
    apt-get autoclean

# Supervisor conf
RUN echo "[supervisord]" >> /etc/supervisor/supervisord.conf && \
    echo "nodaemon = true" >> /etc/supervisor/supervisord.conf && \
    echo "user = root" >> /etc/supervisor/supervisord.conf

# SSH
RUN sed -i "s/Port 22/Port ${SSH_PORT}/" /etc/ssh/sshd_config && \
    sed -i "s/PermitRootLogin yes/PermitRootLogin no/" /etc/ssh/sshd_config && \
    mkdir /var/run/sshd


RUN chown -R root:root /etc/cron.d && \
    chmod -R 0644 /etc/cron.d

ENV LANGUAGE ru_RU.UTF-8
ENV LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8
RUN locale-gen ru_RU.UTF-8 && dpkg-reconfigure locales

COPY service.sh /
RUN chmod 700 /service.sh
CMD ["./service.sh"]
