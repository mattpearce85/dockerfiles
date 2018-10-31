FROM ubuntu:16.04
LABEL Maintainer="Matthew Pearce <mattpearce85@gmail.com>"

RUN apt-get clean \
    && apt-get update -y \
    && apt-get install -y \
        locales \
        curl \
        software-properties-common \
        git \
    && locale-gen en_US.UTF-8 \
    && LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php \
    && apt-get update

