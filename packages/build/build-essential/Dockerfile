#---
# name: build-essential
# group: build
# notes: installs compilers, build tools & configures the default locale
#---
ARG BASE_IMAGE
FROM ${BASE_IMAGE}

ENV DEBIAN_FRONTEND=noninteractive \
    LANGUAGE=en_US:en \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    WGET_FLAGS="--quiet --show-progress --progress=bar:force:noscroll --no-check-certificate"
    #TERM=dumb

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        locales \
        locales-all \
        tzdata \
    && locale-gen en_US $LANG \
    && update-locale LC_ALL=$LC_ALL LANG=$LANG \
    && locale \
    \
    && apt-get install -y --no-install-recommends \
        build-essential \
        software-properties-common \
        apt-transport-https \
        ca-certificates \
        lsb-release \
        pkg-config \
        gnupg \
        git \
        git-lfs \
        gdb \
        wget \
        wget2 \
        curl \
        nano \
        zip \
        unzip \
        time \
        sshpass \
        ssh-client \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    \
    && gcc --version \
    && g++ --version

COPY tarpack vercmp /usr/local/bin/
RUN chmod a+rx /usr/local/bin/tarpack /usr/local/bin/vercmp