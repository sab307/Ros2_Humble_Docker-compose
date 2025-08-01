ARG ROS_DISTRO=jazzy
ARG RMW_ZENOH_BRANCH=jazzy

ARG BASE_IMAGE=ros:${ROS_DISTRO}-ros-core

FROM ${BASE_IMAGE} AS ros_base

# Install required packages to build rmw_zenoh, some demos/tutos for the workshop
# and some usefull tools (vim, ping...)
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-${ROS_DISTRO}-ament-cmake-vendor-package \
    ros-${ROS_DISTRO}-ament-cmake \
    cargo \
    clang \
    ros-${ROS_DISTRO}-ament-cmake-clang-format \
    ros-dev-tools \
    ros-${ROS_DISTRO}-tracetools \
    ros-${ROS_DISTRO}-demo-nodes-cpp \
    ros-${ROS_DISTRO}-demo-nodes-py \
    ros-${ROS_DISTRO}-action-tutorials-cpp \
    ros-${ROS_DISTRO}-action-tutorials-py \
    ros-${ROS_DISTRO}-turtlesim \
    vim \
    iputils-ping iproute2 && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /ros_ws

FROM ros_base AS build

ARG ROS_DISTRO
ARG RMW_ZENOH_BRANCH

SHELL ["/bin/bash", "-c"]
RUN mkdir -p src \
    cd src && \
    git clone https://github.com/ros2/rmw_zenoh.git -b $RMW_ZENOH_BRANCH && \
    cd .. && \
    source /opt/ros/${ROS_DISTRO}/setup.sh && \
    rosdep init && rosdep update

RUN rosdep install --from-paths src --ignore-src --rosdistro ${ROS_DISTRO} -y
SHELL ["/bin/bash", "-c"]
RUN source /opt/ros/${ROS_DISTRO}/setup.sh
RUN colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release

FROM ros_base

COPY --from=build /ros_ws/src /ros_ws/src
COPY --from=build /ros_ws/install /ros_ws/install
COPY rmw_zenoh_env.bash /

# Make /root/.bashrc to load /rmw_zenoh_env.bash for `docker exec` commands to have proper env
RUN echo 'source /rmw_zenoh_env.bash' >> /root/.bashrc

EXPOSE 7447/tcp
EXPOSE 7447/udp

WORKDIR /ros_ws
SHELL ["/bin/bash", "-c"]