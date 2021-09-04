#
# Jugal Kishore -- 2020
# https://github.com/crazyuploader/Docker-Builder
#
# All the dependencies requred for Android Kernel Building
#
# Using Ubuntu as a base OS
FROM ubuntu:focal

# Setting Non-Interactive Build Time Environment Variable
ARG DEBIAN_FRONTEND=noninteractive

# Setting TERM Environment Variable
ENV TERM=xterm-256color

# Updating apt packages list
RUN apt-get update

# Upgrading apt packages to latest
RUN apt-get -y upgrade

# Installing required packages
RUN apt-get install -y \
                    curl git python bc tar make wget gcc libssl-dev \
                    zip unzip ca-certificates gpg gpg-agent ssh \
                    nano vim xz-utils tmux screen jq tree 1> /dev/null

# Install tmate
RUN curl -sL https://raw.githubusercontent.com/crazyuploader/Bash/master/install/tmate.sh | bash -

# Custom Functions
RUN curl -sL https://raw.githubusercontent.com/crazyuploader/Misc/master/functions >> ~/.bashrc

# Removing all unused packages
RUN apt-get -y autoremove --purge

# APT clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/*

# Directory to store cross-compilers
RUN mkdir -p ~/home

# ARM_64 Cross Compiler
RUN printf "Cloning Toolchains and any kernel...\n" && \
    echo "Getting AOSP ARM Cross Compiler..." && \
    cd ~/home && \
    git clone --progress -j 16 --depth 1 --single-branch https://github.com/54b1d/gcc-arm64 -b gcc-master gcc-arm64 && \
    git clone --depth 1 --single-branch https://github.com/54b1d/AnyKernel3 -b tortoise anykernel

# Install ffsend
RUN curl -sL https://raw.githubusercontent.com/crazyuploader/Bash/master/install/ffsend.sh | bash -

# Setting Git Identity
RUN git config --global user.email "sabidul1@gmail.com" && git config --global user.name "54b1d"

# Setting up an environment variable
ENV CROSS_COMPILE ~/home/gcc-arm64/bin/aarch64-elf-

CMD ["bash"]
