CI build and test:

Main: ![Main](https://github.com/dfki-ric/robot_remote_control/actions/workflows/c-cpp.yml/badge.svg)
Devel: ![Devel](https://github.com/dfki-ric/robot_remote_control/actions/workflows/c-cpp.yml/badge.svg?branch=devel)

# Robot Remote Control

This is a library for framework independent remote control of semi-autonomous robots.

This library was initiated and is currently developed at the
[Robotics Innovation Center](http://robotik.dfki-bremen.de/en/startpage.html) of the
[German Research Center for Artificial Intelligence (DFKI)](http://www.dfki.de) in Bremen.



## Motivation

Robots or semi-autonomous vehicles often use their own Framework, with it's own proprietary communication.
While these are often well suited for in-system communication, they often cause trouble when those systems should be controlled by an external connection.

Those connections may also have additional requirements, like low bandwidth, high latency, etc., or custom communication hardware, which does not have an ethernet stack.
This library defines an interface to those robots and supports externally programmed Transports that can handle the requirements above.

## Citing

An evaluation of the library has beed published at the i-SAIRAS conference in 2020, you can find the Paper [here](https://www.hou.usra.edu/meetings/isairas2020fullpapers/pdf/5063.pdf).

If you want to cite this library you can use this bibtex entry:

```bibtex
@inproceedings{danter2020rrc,
    title = {Lightweight and Framework-Independent Communication Library to Support Cross-Plattform Robotic Applications and High-Latency Connections},
    booktitle = {International Symposium on Systems, Artificial Intelligence, Robotics, and Automation in Space (i-SAIRAS), 15th, October 19-23, Online-Conference},
    author = {Leon Cedric Danter and Steffen Planthaber and Alexander Dettmann and Wiebke Brinkmann and Frank Kirchner},
    year = {2020},
    url = {https://www.hou.usra.edu/meetings/isairas2020fullpapers/pdf/5063.pdf}
}
```

## License

[BSD Clause 3](https://opensource.org/licenses/BSD-3-Clause)<br> - Copyright DFKI GmbH

## Installation

### Dependencies

The library is using the high-level networking library ZeroMQ (ZMQ) and Google's language- and platform-neutral proto3 for de-/serialization. Therefor you need to manually install the following OS dependencies: protobuf and zeromq

    sudo apt install libprotobuf-dev protobuf-compiler libzmq3-dev libreadline-dev

### Building

The library is compiled using cmake. To compile it manually execute the following lines from the repositories root directory:

    mkdir build
    cd build
    cmake ..
    make

Optionally you can add the flags -DBUILD_EXAMPLES=ON and -DBUILD_TESTS=ON to build the examples and test directory respectively.

#### Building on systems without protobuf3

In order to install protobuf3 from source you need additional dependencies:

    apt-get install autoconf automake libtool curl make g++ unzip lsb-release

You can use the compile_protobuf_from_source.bash script to build protobuf from source. Optionally you can path the install destination as follows:

    build_protobuf "$INSTALL_PATH"

This will install protobuf 3 in the specified INSTALL_PATH (default is /usr/local)

In case you have a system protobuf installed, defien the protobuf locations by yourself:

    cmake .. -DBUILD_EXAMPLES=true -DPROTOBUF_PROTOC_EXECUTABLE=/bin/protoc -DPROTOBUF_IMPORT_DIRS=/include -DPROTOBUF_INCLUDE_DIR=/include -DPROTOBUF_LIBRARY=/lib/libprotobuf.so.28


### Directory Structure

This directory structure follows some simple rules, to allow for generic build
processes and simplify reuse of this project.

### Folder Structure

| directory         |       purpose                                                  |
| ----------------- | ------------------------------------------------------         |
| src/              | Contains all header (*.hpp) and source (*.cpp) files           |
| build/ *          | The target directory for the build process, temporary content  |
| test/             | contains the boost_test based unit tests                       |
| examples/         | contains demo classes to showcase usage and extensibility      |

## Gettings started

Please have a look into the provided Main examples

* examples/ControlledRobotMain.cpp (Robot that takes the position command and sets its position to it)
* examples/RobotControllerMain.cpp (controller that sets the desired robot position)

You can build the Doxygen documentation using `$> doxygen Doxyfile`, it will be written to the build/doc folder.

There are two main libraries generated when compiling this repository:

### RobotController

The RobotController class is a framework independent cpp class which is able to control any Robot/Vehicle that has a ControlledRobot library wrapped into its control framework.

### ControlledRobot

This library is to be used on the robot, you can map the commands received from the RobotController Library to commands of your Robot to listen to the commands.
Also you can add Telemery to this library, which are then send to the RobotController.


### Transports

Transports are seperated into two directions: commands and telemetry.
While commands from RobotController to ControlledRobot are getting acknowledged on retrieval, the retrieval of telemetry is not confirmed.

These Transports are given to the RobotController and ControlledRobot in their contructor and only implement an send() and receive() function.
So it is easy to implement other means of connections between those by implementing the Transport.hpp interface.


## Testing

The repository includes a test directory with several boost test cases. You can build this directory and the test_suite executable as follows:

    mkdir build
    cd build
    cmake -D -DBUILD_TESTS=ON ..
    make

If you always want to build the test directory as well add the following line to your CMakeLists.txt

    set(BUILD_TESTS ON)

In order to run the tests you can either run all by executing (in /build/test) ```./test_suite``` or choose a specific test with the -t flag ```./test_suite -t checking_current_pose```

## Bug Reports

To search for bugs or report them, please use GitHubs [Issue-Tracker](https://github.com/dfki-ric/robot_remote_control/issues)

## Acknowledgements
This work was mainly developned as part of several projects: 

* ROBDEKON was funded by the Federal Ministry of Economics and Technology in accordance with the parliamentary resolution of the German Parliament (grant no.13N14675). 
* Mare-IT was funded by the German Space Agency (DLR Agentur) with federal funds of the Federal Ministry of Economics and Technology in accordance with the parliamentary resolution of the German Parliament (grant no. 01|S17029A).
* PRO-ACT was funded under the European Commission Horizon 2020 Space Strategic Research Clusters (Operational Grants number 821903)

