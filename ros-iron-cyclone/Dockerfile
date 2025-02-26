# Use the official ROS Jazzy base image
FROM osrf/ros:iron-desktop

# Install CycloneDDS
RUN apt-get update && apt-get install -y \
    ros-iron-rmw-cyclonedds-cpp \
    && rm -rf /var/lib/apt/lists/*

# Set CycloneDDS as the default DDS implementation
ENV RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

# build & Install irobot-create-msgs
RUN mkdir -p /root/create3_ws/src
RUN cd /root/create3_ws/src && \
    git clone -b 2.1.0 https://github.com/iRobotEducation/irobot_create_msgs.git
RUN cd /root/create3_ws && \
    /bin/bash -c "source /opt/ros/iron/setup.bash; colcon build"
    
# Source the ROS setup script
SHELL ["/bin/bash", "-c"]
RUN echo "source /opt/ros/iron/setup.bash" >> ~/.bashrc
RUN echo "source /root/create3_ws/install/setup.bash" >> ~/.bashrc

# Set the entrypoint
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]