# create3-dockers
Docker images for connecting to create3 robots and/or running the simulator.

On Ubuntu, install docker from [apt repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

On Windows, make sure to use docker with your WSL2 image of ubuntu. 

## Building an image
Each image `image-name` is defined in the file `image-name/Dockerfile`. To build it, run 
```
cd image-name
docker build -t image-name .
```

## Starting an image
Since we want to run ROS2, we need network access from the container to the host. This is achieved with the option `--net=host`. For the simulator, we also need display. The command depends whether we have native ubuntu or wsl in Windows. 

If your ROS2 workspace is in `${HOME}/ros2_ws`, it can be mounted in `/root/ros2_ws` using the option `-v ${HOME}/ros2_ws:/root/ros2_ws` 

### Ubuntu with X display
```
docker run -it --net=host --privileged \
            --volume=${HOME}/ros2_ws:/root/ros2_ws \
            --env="DISPLAY=$DISPLAY" \
            --volume="${XAUTHORITY}:/root/.Xauthority" \
            image-name
```

### In Windows+WSL
```
docker run -it --net=host --privileged \
               -v ${HOME}/ros2_ws:/root/ros2_ws \
               -v /tmp/.X11-unix:/tmp/.X11-unix \
               -v /mnt/wslg:/mnt/wslg \
               -e DISPLAY=$DISPLAY \
               -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
               -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
               -e PULSE_SERVER=$PULSE_SERVER \
               image-name
```

### Ubuntu with Wayland (TODO) 

## Open a new terminal for a running image
When working with ROS2, we often need to open multiple terminals. These terminals must of course be attached to the same running image. To attach a new terminal to an image, we must first get its id, using the command `docker ps`, e.g. 
```
$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS     NAMES
b61cc8874769   create3-sim-humble:latest   "/ros_entrypoint.sh …"   13 seconds ago   Up 13 seconds             lucid_goodall
```
Then the idea is to run bash on the this image, e.g., 
```
$ docker exec -it b61cc8874769 bash
```


## Available images
### ros-iron-cyclone
This docker image works with firmware I.0.0 Cyclone dds available at: https://iroboteducation.github.io/create3_docs/releases/i_0_0/


## create3-sim-humble

This contains ros2 humble and the create3 gazebo simulator. The small house is also installed. 

To run a robot in an empty environment, run (in the docker container)
```
ros2 launch irobot_create_gazebo_bringup create3_gazebo.launch.py
```

To run it in the small house:
```
ros2 launch irobot_create_gazebo_bringup create3_gazebo_aws_small.launch.py
```

See https://github.com/iRobotEducation/create3_sim/tree/humble for more details.
