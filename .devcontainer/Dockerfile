FROM osrf/ros:noetic-desktop-full

RUN apt update && export DEBIAN_FRONTEND=noninteractive && \
  apt install -y \
    python3-pip \
    python-is-python3 \
    ros-noetic-four-wheel-steering-msgs \
    ros-noetic-urdf-geometry-parser \
    ros-noetic-moveit \
    ros-noetic-twist-mux-msgs

RUN pip3 install vcstool catkin_tools
