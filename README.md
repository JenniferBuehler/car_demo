# Demo of Prius in ROS/GAZEBO

This is a simulation of a Prius in [gazebo 9](http://gazebosim.org) with sensor data being published using [ROS kinetic](http://wiki.ros.org/kinetic/Installation)
The car's throttle, brake, steering, and gear shifting are controlled by publishing a ROS message.
A ROS node allows driving with a gamepad or joystick.

# Video + Pictures

A video and screenshots of the demo can be seen in this blog post: https://www.osrfoundation.org/simulated-car-demo/

![Prius Image](https://www.osrfoundation.org/wordpress2/wp-content/uploads/2017/06/prius_roundabout_exit.png)

# Requirements

This demo has been tested on Ubuntu Xenial (16.04)

* An X server
* [Docker](https://www.docker.com/get-docker)
* [nvidia-docker2](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))
* The current user is a member of the docker group or other group with docker execution rights.
* [rocker](https://github.com/osrf/rocker)

# Recommended

* A joystick
* A joystick driver which creates links to `/dev/input/js0` or `/dev/input/js1`

This has been tested with the Logitech F710 in Xbox mode. If you have a different joystick you may need to adjust the parameters for the very basic joystick_translator node: https://github.com/osrf/car_demo/blob/master/car_demo/nodes/joystick_translator

# Building

First clone the repo, then run the script `build_demo.bash`.
It builds a docker image with the local source code inside.

```
$ cd car_demo
$ ./build_demo.bash
```

# Running

Connect a game controller to your PC.
Use the script `run_demo.bash` to run the demo.

```
$ ./run_demo.bash
```
An [RVIZ](http://wiki.ros.org/rviz) window will open showing the car and sensor output.
A gazebo window will appear showing the simulation.
Either use the controller to drive the prius around the world, or click on the gazebo window and use the `WASD` keys to drive the car.

If using a Logitech F710 controller:

* Make sure the MODE status light is off
* Set the swtich to XInput mode
* The right stick controls throttle and brake
* The left stick controls steering
* Y puts the car into DRIVE
* A puts the car into REVERSE
* B puts the car into NEUTRAL

# Running directly without Docker

If you have ROS already set up on your system, there is no need to
go via docker, you can run the demo directly.

You will need to build the [citysim](https://bitbucket.org/osrf/citysim) package for Gazebo first.
Then add this repository to your catkin workspace and build as usual.
Then launch with 

`roslaunch car_demo demo.launch`

If you don't have a joystick, and want to use keyboard control
(and avoid errors due to no joystick found), use

`roslaunch car_demo demo.launch use_joystick:=false`

## Keyboard control

There are two modes, default is Mode B.
Unfortunately at this point switching of modes is not supported, you 
have to set the default value of `keyControl` in
`car_demo/plugins/PriusHybridPlugin.cc` and re-compile.

### Mode A

* e - gas pedal
* w - release pedals
* q - brake
* a - steer left
* d - steer right
* s - center steering
* z - reverse
* x - neutral
* c - forward

### Mode B

* w - accelerate forward
* a - steer left
* s - reverse
* d - steer right
* e - brake
* x - neutral
* q - EV mode
