FROM ubuntu:18.04

LABEL maintainer="goat <ryantgray@gmail.com>"

RUN apt-get update \
    && apt-get -y install wget lsb-release gnupg

RUN wget http://packages.ntop.org/apt/18.04/all/apt-ntop.deb \
    && dpkg -i apt-ntop.deb \
    && rm -rf apt-ntop.deb

RUN apt-get update && apt-get -y install pfring-dkms ntopng n2disk cento iputils-ping \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY ntopng.conf /etc/ntopng/ntopng.conf
RUN touch /etc/ntopng/ntopng.start

EXPOSE 3000

ENTRYPOINT ["ntopng"]
