#
# Jugal Kishore -- 2020
# https://github.com/crazyuploader/Docker-Builder
#
# All the dependencies requred for Android Kernel Building
#
# Using Ubuntu as a base OS
FROM ubuntu:focal

#Setup enviroinment from apon77
RUN /bin/sh -c set -xe      && echo '#!/bin/sh' > /usr/sbin/policy-rc.d     && echo 'exit 101' >> /usr/sbin/policy-rc.d     && chmod +x /usr/sbin/policy-rc.d       && dpkg-divert --local --rename --add /sbin/initctl     && cp -a /usr/sbin/policy-rc.d /sbin/initctl    && sed -i 's/^exit.*/exit 0/' /sbin/initctl         && echo 'force-unsafe-io' > /etc/dpkg/dpkg.cfg.d/docker-apt-speedup         && echo 'DPkg::Post-Invoke { "rm -f /var/cache/apt/archives/*.deb /var/cache/apt/archives/partial/*.deb /var/cache/apt/*.bin || true"; };' > /etc/apt/apt.conf.d/docker-clean   && echo 'APT::Update::Post-Invoke { "rm -f /var/cache/apt/archives/*.deb /var/cache/apt/archives/partial/*.deb /var/cache/apt/*.bin || true"; };' >> /etc/apt/apt.conf.d/docker-clean   && echo 'Dir::Cache::pkgcache ""; Dir::Cache::srcpkgcache "";' >> /etc/apt/apt.conf.d/docker-clean      && echo 'Acquire::Languages "none";' > /etc/apt/apt.conf.d/docker-no-languages      && echo 'Acquire::GzipIndexes "true"; Acquire::CompressionTypes::Order:: "gz";' > /etc/apt/apt.conf.d/docker-gzip-indexes       && echo 'Apt::AutoRemove::SuggestsImportant "false";' > /etc/apt/apt.conf.d/docker-autoremove-suggests

#
RUN /bin/sh -c mkdir -p /run/systemd && echo 'docker' > /run/systemd/container
CMD ["/bin/bash"]
RUN /bin/sh -c git config --global user.name ci && git config --global user.email ci@ci.ci && git config --global color.ui auto
ENV USER=ci
ENV HOSTNAME=ci

# Setting Non-Interactive Build Time Environment Variable
ARG DEBIAN_FRONTEND=noninteractive

# Setting TERM Environment Variable
ENV TERM=xterm-256color

# Updating apt packages list
RUN apt-get update && apt-get -y upgrade

# Installing required packages
RUN apt-get install -y \
                    git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig git python bc tar make wget gcc libssl-dev ca-certificates gpg gpg-agent ssh nano vim xz-utils tmux rsync screen jq tree locales 1> /dev/null

RUN /bin/sh -c sudo apt autoremove && sudo apt clean

# Fix locale issues
RUN echo echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

# Removing all unused packages
RUN apt-get -y autoremove --purge

# APT clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/*

# Directory to store cross-compilers
RUN mkdir -p ~/home


# Setting Git Identity
RUN git config --global user.email "ci@cirrus-ci.com" && git config --global user.name "ci"

ENV LC_CTYPE en_US.UTF-8
ENV LC_ALL=C

CMD ["bash"]
