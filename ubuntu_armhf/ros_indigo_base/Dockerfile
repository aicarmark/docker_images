FROM armhf/ubuntu:trusty
MAINTAINER tao.tan

# support to run on x86 host by qemu
COPY qemu-arm-static /usr/bin/qemu-arm-static

# avoid debconf error out
# https://github.com/docker/docker/issues/4032
# https://stackoverflow.com/questions/22466255/is-it-possibe-to-answer-dialog-questions-when-installing-under-docker
ARG TERM=xterm
ARG DEBIAN_FRONTEND=noninteractive

# gcc-4.9/g++-4.9  full support c++11
RUN echo "deb http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu trusty main" > \
    /etc/apt/sources.list.d/ubuntu-toolchain-r-test-trusty.list
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 1E9377A2BA9EF27F
RUN apt-get update && apt-get install -y \
    gcc-4.9 \
    g++-4.9 \
    && rm -rf /var/lib/apt/lists/*
RUN ln -fs /usr/bin/gcc-4.9 /usr/bin/gcc \
    && ln -fs /usr/bin/g++-4.9 /usr/bin/g++

# ros-indigo-base
ENV LANG C.UTF-8 
ENV LANGUAGE C.UTF-8 
ENV LC_ALL C.UTF-8 
ENV LC_MESSAGES POSIX
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116
RUN echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list
RUN apt-get update && apt-get install -y \
    ros-indigo-ros-base \
    python-rosdep \
    && rm -rf /var/lib/apt/lists/*
RUN rosdep init && rosdep update
RUN echo "source /opt/ros/indigo/setup.bash" >> /root/.bashrc

# PS1
RUN echo "export PS1=\"\[\033[38;5;2m\][\[\$(tput sgr0)\]\[\033[38;5;202m\]\u\[\$(tput sgr0)\]\[\033[38;5;2m\]@\[\$(tput sgr0)\]\[\033[38;5;27m\]\w\[\$(tput sgr0)\]\[\033[38;5;2m\]]\[\$(tput sgr0)\]\[\033[38;5;202m\]\\\$\[\$(tput sgr0)\]\[\033[38;5;15m\] \[\$(tput sgr0)\]\""  >> /root/.bashrc

CMD ["/bin/bash"]
