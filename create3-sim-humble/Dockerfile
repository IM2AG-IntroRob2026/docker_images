# Use the official ROS humble base image
FROM osrf/ros:humble-desktop

# Install irobot create msgs
RUN apt-get update && apt-get install -y \
    ros-humble-irobot-create-msgs \
    python3-pip \
    gazebo 

# get create3_sim repo
RUN mkdir -p /root/create3_ws/src && \ 
    cd /root/create3_ws/src && \
    git clone -b humble https://github.com/iRobotEducation/create3_sim.git 

# get dependencies
RUN cd /root/create3_ws && \
    /bin/bash -c "source /opt/ros/humble/setup.bash; rosdep install --from-path src -yi" \
    && rm -rf /var/lib/apt/lists/*

# install the small house    
RUN vcs import ~/create3_ws/src/ < ~/create3_ws/src/create3_sim/irobot_create_gazebo/demo.repos

# build the workspace
RUN cd /root/create3_ws && \
    /bin/bash -c "source /opt/ros/humble/setup.bash; colcon build --symlink-install"

# Source the ROS setup script
SHELL ["/bin/bash", "-c"]
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
RUN echo "source /root/create3_ws/install/local_setup.bash" >> ~/.bashrc

# Set the entrypoint
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]