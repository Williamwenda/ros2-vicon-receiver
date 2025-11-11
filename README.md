# Vicon wrapper for ROS2

This ros2 package can be used to stream data from a Vicon mocap system and publish it to custom ROS2 topics.
This repository has the following changes from the [original code base](https://github.com/OPT4SMART/ros2-vicon-receiver):
- Pose information is broadcast as a tf2

If you are using this repo and encounter any problems please report an issue! 

## Network setup

This repo. works with the Vicon system in UTIAS, room 195. Connect to the following WiFi

Wifi: DSL_DroneNet_5G

Passwd: He!!0_ARDrones

The Vicon ip address (hostname) is: '192.168.2.119:801'

## Vicon DataStream SDK

Get the official binaries released in the official download page [here](https://www.vicon.com/software/datastream-sdk/?section=downloads).

You can check the documentation [here](https://docs.vicon.com/spaces/viewspace.action?key=DSSDK19).

### Installing on Linux

Copy all the libraries to /usr/local/lib and move the headers to /usr/local/include/ViconDataStreamSDK/.

```
sudo mv {YOUR_ViconDataStreamSDK}/Linux64/Release/* /usr/local/lib/
cd /usr/local/include/
sudo mkdir ViconDataStreamSDK
sudo mv ../lib/*.h ViconDataStreamSDK/
```

Update the `LD_LIBRARY_PATH` environment variable:

`export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/`

It's convenient to automatically update this environment variable in your bash session every time a new shell is launched, so type it into your .bashrc file:

`echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/"`

## Quick start

### Building the package

:warning: Do not forget to source the ROS2 workspace: `source /opt/ros/dashing/setup.bash`

Enter the project folder and build the executable
```
$ cd vicon_receiver
$ colcon build --symlink-install
```

### Running the program

Open a new terminal and source the project workspace:
```
$ source vicon_receiver/install/setup.bash
```

To run the program, use the [launch file template](vicon_receiver/launch/client.launch.py) provided in the package. First, open the file and edit the parameters. Running `colcon build` is not needed because of the `--symlink-install` option previously used.

Now you can the program with
```
$ ros2 launch vicon_receiver client.launch.py
```

Exit the program with CTRL+C.

### Information on ROS2 topics and messages

The **ros2-vicon-receiver** package creates a topic for each segment in each subject with the pattern `namespace/subject_name/segment_name`. Information is published on the topics as soon as new data is available from the vicon client (typically at the vicon client frequency). The message type [Position](vicon_receiver/msg/Position.msg) is used.

Example: suppose your namespace is the default `vicon` and you have two subjects (`subject_1` and `subject_2`) with two segments each (`segment_1` and `segment_2`). Then **ros2-vicon-receiver** will publish [Position](vicon_receiver/msg/Position.msg) messages on the following topics:
```
vicon/subject_1/segment_1
vicon/subject_1/segment_2
vicon/subject_2/segment_1
vicon/subject_2/segment_2
```

## Constributors
**ros2-vicon-receiver** is developed by
[Andrea Camisa](https://www.unibo.it/sitoweb/a.camisa),
[Andrea Testa](https://www.unibo.it/sitoweb/a.testa) and
[Giuseppe Notarstefano](https://www.unibo.it/sitoweb/giuseppe.notarstefano)

## Acknowledgements
This result is part of a project that has received funding from the European Research Council (ERC) under the European Union's Horizon 2020 research and innovation programme (grant agreement No 638992 - OPT4SMART).

<p style="text-align:center">
  <img src="logo_ERC.png" width="200" />
  <img src="logo_OPT4Smart.png" width="200" /> 
</p>
