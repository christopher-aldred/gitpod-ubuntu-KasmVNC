# FROM ubuntu:20.04
FROM ubuntu:20.04
# LABEL org.opencontainers.image.title="Traversaro's Ubuntu 20.04 blank image with VNC support"
# LABEL org.opencontainers.image.description="Infrastructure for running grasping experiments with the iCub humanoid in Gazebo"
# LABEL org.opencontainers.image.source="https://github.com/traversaro/gitpod-ubuntu-20.04"
# LABEL org.opencontainers.image.authors="Silvio Traversaro <silvio.traversaro@iit.it>"

# Non-interactive installation mode
ENV DEBIAN_FRONTEND=noninteractive

# Update apt database
RUN apt update

# Install essentials
RUN apt install -y apt-utils software-properties-common apt-transport-https sudo \
    psmisc tmux nano wget curl telnet gnupg gdb git gitk autoconf locales gdebi \
    terminator meld dos2unix meshlab

# Set the locale
RUN locale-gen en_US.UTF-8

# Install graphics
RUN apt install -y xfce4 xfce4-goodies xserver-xorg-video-dummy xserver-xorg-legacy x11vnc firefox && \
    apt remove -y xfce4-power-manager light-locker && \
    sed -i 's/allowed_users=console/allowed_users=anybody/' /etc/X11/Xwrapper.config
COPY dockerfiles/xorg.conf /etc/X11/xorg.conf
RUN dos2unix /etc/X11/xorg.conf

# Install python
RUN apt install -y python3 python3-dev python3-pip python3-setuptools
    
# Install other dependencies
RUN apt-get install -y bash-completion build-essential cmake cmake-curses-gui coinor-libipopt-dev curl freeglut3-dev git libace-dev libboost-filesystem-dev libboost-system-dev libboost-thread-dev libdc1394-22-dev libedit-dev libeigen3-dev libgsl0-dev libjpeg-dev liblua5.1-dev libode-dev libopencv-dev libsdl1.2-dev libtinyxml-dev libv4l-dev libxml2-dev lua5.1 portaudio19-dev qml-module-qt-labs-folderlistmodel qml-module-qt-labs-settings qml-module-qtmultimedia qml-module-qtquick-controls qml-module-qtquick-dialogs qml-module-qtquick-window2 qml-module-qtquick2 qtbase5-dev qtdeclarative5-dev qtmultimedia5-dev swig libmatio-dev libirrlicht-dev libspdlog-dev libblas-dev liblapack-dev wget sudo nlohmann-json3-dev zsh xonsh ninja-build 

# Set environmental variables
ENV DISPLAY=:1

# Create user gitpod
RUN useradd -l -u 33333 -G sudo -md /home/gitpod -s /bin/bash -p gitpod gitpod && \
    # passwordless sudo for users in the 'sudo' group
    sed -i.bkp -e 's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' /etc/sudoers

# Switch to gitpod user
USER gitpod

# Create the Desktop dir
RUN mkdir -p /home/gitpod/Desktop

# Switch back to root
USER root

# Please choose the package for your distro here (under Assets):
# https://github.com/kasmtech/KasmVNC/releases
RUN wget https://github.com/kasmtech/KasmVNC/releases/download/v1.3.2/kasmvncserver_focal_1.3.2_amd64.deb

RUN apt-get install -y ./kasmvncserver_focal_1.3.2_amd64.deb

RUN usermod -a -G ssl-cert gitpod

# # YOU MUST DISCONNECT AND RECONNECT FOR GROUP MEMBERSHIP CHANGE TO APPLY

# # start KasmVNC, you will be prompted to create a KasmVNC user and select a desktop environment
# RUN vncserver

# # Tail the logs
# RUN tail -f ~/.vnc/*.log

# Manage ports
EXPOSE 5901 6080 10000/tcp 10000/udp

# Clean up unnecessary installation products
RUN rm -Rf /var/lib/apt/lists/*

# Launch bash from /workspace
WORKDIR /workspace
CMD ["bash"]
