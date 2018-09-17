FROM ubuntu:18.04

LABEL maintainer="goat <ryantgray@gmail.com>"

RUN apt-get update \
    && apt-get -y install wget lsb-release gnupg

RUN wget http://packages.ntop.org/apt/18.04/all/apt-ntop.deb \
    && dpkg -i apt-ntop.deb \
    && rm -rf apt-ntop.deb

RUN apt-get update && apt-get -y install nprobe iputils-ping \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY nprobe-none.conf /etc/nprobe/nprobe-none.conf
RUN touch /etc/nprobe/nprobe-none.start

EXPOSE 2055/udp
EXPOSE 5556

ENTRYPOINT ["nprobe"]
