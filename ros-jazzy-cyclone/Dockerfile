# Use the official ROS Jazzy base image
FROM osrf/ros:jazzy-desktop

# Install CycloneDDS
RUN apt-get update && apt-get install -y \
    ros-jazzy-rmw-cyclonedds-cpp \
    && rm -rf /var/lib/apt/lists/*

# Set CycloneDDS as the default DDS implementation
ENV RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

# Source the ROS setup script
SHELL ["/bin/bash", "-c"]
RUN echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc

# Set the entrypoint
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]