FROM ubuntu:18.04

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
  mercurial autoconf build-essential libtool \
  && rm -rf /var/lib/apt/lists/*

RUN hg clone http://hg.code.sf.net/p/etherlabmaster/code /home/ethercat-hg

WORKDIR /home/ethercat-hg
RUN hg update stable-1.5

RUN ./bootstrap
RUN ./configure --enable-sii-assign --disable-8139too --enable-hrtimer --enable-cycles --disable-kernel
RUN make all
RUN make install
RUN ldconfig
