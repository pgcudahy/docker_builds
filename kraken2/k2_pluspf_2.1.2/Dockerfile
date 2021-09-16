#Forked from https://github.com/StaPH-B/docker-builds/blob/master/kraken2/2.1.2/Dockerfile
FROM ubuntu:xenial

# for easy upgrade later. ARG variables only persist during build time.
ARG K2VER="2.1.2"

LABEL base.image="ubuntu:xenial"
LABEL dockerfile.version="1"
LABEL software="Kraken2"
LABEL software.version="2.1.2"
LABEL description="Taxonomic sequence classifier"
LABEL website="https://github.com/DerrickWood/kraken2"
LABEL license="https://github.com/DerrickWood/kraken2/blob/master/LICENSE"
LABEL maintainer="Patrick Cudahy"
LABEL maintainer.email="patrick.cudahyh@yale.edu"

# install dependencies and cleanup apt garbage
RUN apt-get update && apt-get -y --no-install-recommends install \
 wget \
 ca-certificates \
 zlib1g-dev \
 make \
 g++ \
 rsync \
 cpanminus && \
 rm -rf /var/lib/apt/lists/* && apt-get autoclean

# DL Kraken2, unpack, and install
RUN wget https://github.com/DerrickWood/kraken2/archive/v${K2VER}.tar.gz && \
 tar -xzf v${K2VER}.tar.gz && \
 rm -rf v${K2VER}.tar.gz && \
 cd kraken2-${K2VER} && \
 ./install_kraken2.sh . && \
 mkdir /data /kraken2-db

ENV PATH="$PATH:/kraken2-${K2VER}" \
    LC_ALL=C

WORKDIR /data

# Get database
# --strip-components=1 used so that the *.k2d files end up inside /kraken2-db and not another directory
RUN cd /kraken2-db && \
 wget --no-verbose --continue https://genome-idx.s3.amazonaws.com/kraken/k2_pluspf_20210517.tar.gz && \
 tar xzvf --strip-components=1 k2_pluspf_20210517.tar.g
 rm -rf k2_pluspf_20210517.tar.gz
