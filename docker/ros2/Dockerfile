# https://hub.docker.com/_/ros
#FROM ros:galactic

# https://github.com/osrf/docker_images/blob/master/README.md#ros---1
# https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/desktop/Dockerfile
FROM ghcr.io/sloretz/ros:jazzy-desktop-full

ARG user_id=1001
ARG ros_ws=/home/dockeruserjazzy/dev_ws

ENV DEBIAN_FRONTEND=noninteractive

# Install packages.
RUN apt-get update && apt-get install -y \
    ros-${ROS_DISTRO}-gazebo-* \
    ros-${ROS_DISTRO}-rmw-cyclonedds-cpp \
    ros-${ROS_DISTRO}-nav2-*\
    ros-${ROS_DISTRO}-turtlebot3-gazebo\
    ros-${ROS_DISTRO}-turtlebot3-navigation2\
    python-is-python3 \
    nano \
    less \
    xterm \
    net-tools \
    iputils-ping
#    && rm -rf /var/lib/apt/lists/*

# Switch to a non-root user.
# RUN useradd -m --uid ${user_id} dockeruserjazzy
# USER dockeruserjazzy

# Set up environment for TurtleBot3 (replace "burger" with your model)
ENV TURTLEBOT3_MODEL burger

# Working directory
WORKDIR /home/dockeruserjazzy
COPY ./cyclonedds_config.xml /home/dockeruserjazzy/cyclonedds_config.xml


# ENVs
ENV HOME=/home/dockeruserjazzy
ENV PATH="/home/dockeruserjazzy/.local/bin:${PATH}"

# Force Python stdout and stderr streams to be unbuffered.
ENV PYTHONUNBUFFERED 1

# Add sourcing to your shell startup script
# https://docs.ros.org/en/galactic/Tutorials/Configuring-ROS2-Environment.html#add-sourcing-to-your-shell-startup-script
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

# Add `colcon_cd` to your shell startup script
# https://docs.ros.org/en/galactic/Tutorials/Configuring-ROS2-Environment.html#add-colcon-cd-to-your-shell-startup-script
RUN echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc \
    && echo "export _colcon_cd_root=${ros_ws}" >> ~/.bashrc

# This entrypoint is from base image.
# See https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/ros-core/ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]

CMD ["sleep", "infinity"]
