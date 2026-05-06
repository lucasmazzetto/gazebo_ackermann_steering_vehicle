# Gazebo Ackermann Steering Vehicle

This package, built with ROS 2 Jazzy Jalisco and Gazebo Harmonic, launches a simulation of an Ackermann steering vehicle. The vehicle model includes steering angle and velocity control, along with an embedded front camera that streams live images for vision-based tasks. This setup could be used for developing and testing autonomous driving algorithms in a simulated environment. 

![vehicle simulation](https://github.com/user-attachments/assets/1726cb9f-d4c0-4b1a-90e4-4b3c395b9268)

For the documentation on the development of this package, including setup, modeling, and simulation steps, please refer to the full guide here: [Ackermann Steering Vehicle Simulation](https://workabotic.com/2025/ackermann-steering-vehicle-simulation/).

## 💻 Installation

Clone this repository into your ```workspace/src``` folder. If you don't have a workspace set up, you can learn more about creating one in the [ROS 2 workspace tutorial](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html).

```bash
cd <path_to_your_workspace>/src

git clone git@github.com:lucasmazzetto/gazebo_ackermann_steering_vehicle.git
```

### 🐧 Linux Setup

This project is designed to run on Linux Ubuntu 24.04 and may also work on other Linux versions or distributions, although additional adjustments might be required. 

#### 📚 Requirements

To use this package, you'll need the following:

- [Linux Ubuntu 24.04](https://ubuntu.com/blog/tag/ubuntu-24-04-lts)
- [ROS2 Jazzy Jalisco](https://docs.ros.org/en/rolling/Releases/Release-Jazzy-Jalisco.html)
- [Gazebo Harmonic](https://gazebosim.org/docs/harmonic/getstarted/)

**Make sure to install the following ROS 2 Jazzy Jalisco packages:**

```bash
sudo apt update
sudo apt install -y \
     ros-jazzy-ros2-controllers \
     ros-jazzy-ros2-control \
     ros-jazzy-gz-ros2-control \
     ros-jazzy-ros-gz \
     ros-jazzy-ros-gz-bridge \
     ros-jazzy-joint-state-publisher \
     ros-jazzy-robot-state-publisher \
     ros-jazzy-xacro \
     ros-jazzy-joy                           
```

#### 🛠️ Build

Source the ROS 2 environment and build the package:

```bash
source /opt/ros/jazzy/setup.bash

cd <path_to_your_workspace>

colcon build
```

After a successful build, the package is ready to be used.

### 🐳 Docker Setup

**[Docker](https://www.docker.com/)** is required to build and run this project using the Docker setup. It ensures a consistent environment and simplifies dependency management across different systems. Make sure Docker is properly installed and running on your machine before proceeding with the build and execution steps.

#### 🛠️ Build

Navigate to the project directory and run the `build_docker.sh` script to build the Docker container. Make sure you're inside the folder, as the script depends on local files. The build may take a while, depending on your internet speed and system performance.

```bash
cd <path_to_your_workspace>/src/gazebo_ackermann_steering_vehicle

./build_docker.sh
```

#### 🏃 Run

To run the Docker container, make sure you're in the project directory and execute the `run_docker.sh` script. This will open a command line with the environment fully configured. You can run this script as many times as needed, and each time it will launch a new command line session inside the Docker container, ready to execute commands within the configured environment.

```bash
cd <path_to_your_workspace>/src/gazebo_ackermann_steering_vehicle

./run_docker.sh
```

For instance, if you need to run the simulation, simply execute the `run_docker.sh` script to open a command line inside the Docker container and execute the following command:

```bash
cd <path_to_your_workspace>/src/gazebo_ackermann_steering_vehicle

./run_docker.sh

ros2 launch gazebo_ackermann_steering_vehicle vehicle.launch.py
```

To use the joystick, open a new command line inside the Docker container by running the `run_docker.sh` script again in a new terminal. Once inside, execute the following command:

```bash
cd <path_to_your_workspace>/src/gazebo_ackermann_steering_vehicle

./run_docker.sh

ros2 launch gazebo_ackermann_steering_vehicle joystick.launch.py
```

## 🚀 Usage

### 🚗 Launch the Vehicle

After building the package, launch the ```vehicle.launch.py``` file from the ```gazebo_ackermann_steering_vehicle``` package:

```bash
source /opt/ros/jazzy/setup.bash

cd <path_to_your_workspace>

source install/setup.bash

ros2 launch gazebo_ackermann_steering_vehicle vehicle.launch.py
```

To launch the robot in a specified world with a custom initial pose, run the ```vehicle.launch.py``` file and specify the world path and robot pose arguments.

- **world**: Path to the world file
- **x**: Initial x-coordinate of the robot
- **y**: Initial y-coordinate of the robot
- **z**: Initial z-coordinate of the robot
- **R**: Initial roll orientation
- **P**: Initial pitch orientation
- **Y**: Initial yaw orientation

In the following example, the robot starts at position (x, y, z) = (1.0, 2.0, 0.5) with a yaw of 1.57 radians in the specified world:

```bash
ros2 launch gazebo_ackermann_steering_vehicle vehicle.launch.py world:=/path_to_world/world.sdf x:=1.0 y:=2.0 z:=0.5 R:=0.0 P:=0.0 Y:=1.57
```

### 🎮 Control

#### Topics

The vehicle can be controlled by publishing the steering angle in radians and velocity in meters per second to the respective topics:

```bash
/steering_angle
/velocity
```

The following topics can be subscribed to access the vehicle's odometry & imu sensor data and retrieve its information:
```bash
/odom
/imu
```

The following topics can be subscribed to access the camera image and retrieve its information:

```bash
/camera/image_raw
/camera/info
```

#### 🕹️ Joystick

To control the vehicle, you can use a video game joystick by launching ```joystick.launch.py```. This launch file starts the joystick_controller node, designed specifically for compatibility with an Xbox One joystick, to interface with the ```velocity``` and ```steering_angle``` topics.

To launch the ```joystick.launch.py```, run the following commands:

```bash
source /opt/ros/jazzy/setup.bash

cd <path_to_your_workspace>

source install/setup.bash

ros2 launch gazebo_ackermann_steering_vehicle joystick.launch.py
```

## ⚙️ Parameters

The parameters for the vehicle model, control, and camera can be configured in the ```gazebo_ackermann_steering_vehicle/config/parameters.yaml``` file. This file includes the following settings with their default values for the simulation:

```yaml
# Body params
body_length: 0.3 # Length of the vehicle's body [m]
body_width: 0.18 # Width of the vehicle's body [m]
body_height: 0.05 # Height of the vehicle's body [m]
body_density: 7850.0 # Density of the vehicle's body material, e.g., steel [kg/m^3]

# Wheel params
wheel_radius: 0.04 # Radius of each wheel [m]
wheel_width: 0.02 # Width of each wheel [m]
wheel_density: 900.0 # Density of the wheel material, e.g., rubber [kg/m^3]

# Kinematics and dynamics params
max_steering_angle: 0.6108652 # Maximum steering angle of the vehicle [rad]
max_steering_angular_velocity: 1.570796 # Maximum steering angular velocity [rad/s]
max_steering_effort: 1.0 # Maximum steering torque [Nm]
max_velocity: 2.0 # Maximum wheel velocity [m/s]
max_effort: 10.0 # Maximum wheel torque [Nm]

# Camera and image params
camera_box_size: 0.05 # Size of the camera enclosure [m]
camera_stick_size: 0.02 # Size of the camera stick [m]
camera_height: 0.2 # Height of the camera above the body_link [m]
camera_pitch: 0.698131 # Pitch angle of the camera relative to body_link [rad]
camera_fov: 1.3962634 # Field of view of the camera [rad]
camera_fps: 30 # Frames per second for camera capture [Hz]
image_width: 640 # Width of the camera's image output [pixels]
image_height: 480 # Height of the camera's image output [pixels]
```
