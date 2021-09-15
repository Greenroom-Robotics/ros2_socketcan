# AutowareAuto - ROS2 SocketCAN
This package was forked from the autoware.auto repo. there are not many docs around but have been able to deduce the following:

- [AutowareAuto - ROS2 SocketCAN](#autowareauto---ros2-socketcan)
- [Build](#build)
- [Usage](#usage)
  - [Setup VCAN and Run Node](#setup-vcan-and-run-node)
  - [CLI Tools](#cli-tools)
  - [Extra Info](#extra-info)

# Build
```
$ colcon build
$ source ./install/setup.bash
```

# Usage
## Setup VCAN and Run Node
1. First setup the virtual can network by running the setup script `bash ./test/vcan0_setup.sh`
    - This will setup a virtual can network `vcan0`
2. Start the can bridge using `vcan0`
```
$ ros2 launch ros2_socketcan socket_can_bridge.launch.xml interface:=vcan0 receiver_interval_sec:=0.1
```

## CLI Tools
`can-utils` apt package provides additional tools to can testing and usage.
```
$ sudo apt update
$ sudo apt install can-utils
```

**Sending messages**

To send a message, open a new terminal and run `cansend vcan 213##311`, see `cansend --h` for further options.

**Receiving messages**

To view the CANbus messages, open a new terminal and run `candump vcan0` to view all messages on the bus.

## Extra Info
To setup a tunnel vxcan network, you can use the following to setup vxcan0 and vxcan1

```
$ sudo ip link add vxcan0 type vxcan peer name vxcan1
$ sudo ip link set vxcan0 up
$ sudo ip link set vxcan1 up
```

*Note: If running a vxcan tunnelled setup, a device will not recieve it's own messages on it's own bus, ie, using `cansend vxcan0...` will not be visible on that bus, ie `candump vxcan0`*