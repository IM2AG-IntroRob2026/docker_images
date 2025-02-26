# create3-dockers
Docker images for connecting to create3 robots and/or running the simulator.

## ros-iron-cyclone

This docker image works with firmware I.0.0 Cyclone dds available at:

https://iroboteducation.github.io/create3_docs/releases/i_0_0/

To build the image, run 

```
cd ros-iron-cyclone
docker build -t ros-iron-cyclone .
```

## create3-sim-humble

This contains ros2 humble and the create3 gazebo simulator. 

To build the image, run 

```
cd create3-sim-humble
docker build -t create3-sim-humble .
```

