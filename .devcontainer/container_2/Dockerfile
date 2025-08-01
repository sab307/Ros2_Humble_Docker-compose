FROM ubuntu:22.04 AS base

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Install language
RUN apt-get update && apt-get install -y \
  locales \
  && locale-gen en_US.UTF-8 \
  && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
  && rm -rf /var/lib/apt/lists/*
ENV LANG en_US.UTF-8

# Install timezone
RUN ln -fs /usr/share/zoneinfo/UTC /etc/localtime \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y tzdata \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && rm -rf /var/lib/apt/lists/*

# install packages
RUN apt-get update && apt-get install -q -y \
  lsb-release \
  wget \
  gnupg \
  sudo 

RUN  wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
  
RUN apt-get update && apt-get install -y -q \
 ignition-fortress
RUN rm -rf /var/lib/apt/lists/*

###########################################
# Develop image 
###########################################
#FROM base AS dev

#ENV DEBIAN_FRONTEND=noninteractive
# Install dev tools
RUN apt-get update && apt-get install -y \
  libignition-plugin-dev \
  libignition-cmake2-dev \
  libignition-gazebo7-dev \
  git \
  vim \
  && rm -rf /var/lib/apt/lists/*

#ARG USERNAME=ubuntu
#ARG USER_UID=1000
#ARG USER_GID=$USER_UID

# Create a non-root user
#RUN groupadd --gid $USER_GID $USERNAME \
  #&& useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
  # [Optional] Add sudo support for the non-root user
  #&& apt-get update \
  #&& apt-get install -y sudo \
  #&& echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
  #&& chmod 0440 /etc/sudoers.d/$USERNAME \
  # Cleanup
  #&& rm -rf /var/lib/apt/lists/* \
  #&& echo "source /usr/share/bash-completion/completions/git" >> /home/$USERNAME/.bashrc \
  #&& echo "if [ -f /opt/ros/${ROS_DISTRO}/setup.bash ]; then source /opt/ros/${ROS_DISTRO}/setup.bash; fi" >> /home/$USERNAME/.bashrc

