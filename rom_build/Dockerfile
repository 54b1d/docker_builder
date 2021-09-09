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
RUN apt-get update && apt-get -y upgrade

# Installing required packages
RUN apt-get install -y git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip python python2.7 android-tools-adb android-tools-fastboot locales 1> /dev/null

# Installing python-pip
RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py && python get-pip.py

# Removing all unused packages
RUN apt-get -y autoremove --purge

# APT clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/*

# Directory to store cross-compilers
RUN mkdir -p ~/home

# Setting Git Identity
RUN git config --global user.email "ci@cirrus-ci.com" && git config --global user.name "ci" && git config --global color.ui auto

# Fix locale issues
RUN echo echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen
ENV LC_CTYPE en_US.UTF-8
ENV LC_ALL=C

CMD ["bash"]
