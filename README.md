# ROS 2 Differential-Drive Robot

A compact ROS 2 and Gazebo Sim project demonstrating how to describe,
visualize, and simulate a custom mobile robot. The workspace brings together a
modular Xacro robot model, ROS 2 launch files, an RViz configuration, and the
bridges needed to connect ROS topics with the simulator.

## What this project demonstrates

- Building a robot model from reusable Xacro files, including links, joints,
  visual and collision geometry, materials, and inertial properties.
- Describing a simple differential-drive platform with two powered wheels and
  a caster wheel.
- Publishing the robot model and its transforms with `robot_state_publisher`.
- Inspecting the robot and its frames in RViz.
- Spawning the model in Gazebo Sim and bridging velocity commands, joint states,
  and transforms between Gazebo and ROS 2.

The robot model uses a rectangular base (0.6 m × 0.4 m × 0.2 m), two drive
wheels (0.1 m radius), and a spherical caster. Its dimensions and geometry are
defined in the Xacro files and can be adapted for experiments.

## Packages

| Package | Purpose |
| --- | --- |
| `my_robot_description` | Robot model, Gazebo-specific robot settings, RViz configuration, and a launch setup for viewing the model. |
| `my_robot_bringup` | Gazebo simulation launch file and ROS–Gazebo topic bridge configuration. |

## Repository layout

```text
src/
├── my_robot_bringup/
│   ├── config/
│   │   └── gazebo_bridge.yaml
│   └── launch/
│       └── my_robot.launch.xml
└── my_robot_description/
    ├── launch/
    │   ├── display.launch.py
    │   └── display.launch.xml
    ├── rviz/
    │   └── urdf_config.rviz
    └── urdf/
        ├── common_properties.xacro
        ├── mobile_base.xacro
        ├── mobile_base_gazebo.xacro
        └── my_robot.urdf.xacro
```

## Requirements

Use a ROS 2 installation that is compatible with Gazebo Sim and the `ros_gz`
packages. The workspace relies on:

- `xacro`
- `robot_state_publisher`
- `joint_state_publisher_gui`
- `rviz2`
- `ros_gz_sim`
- `ros_gz_bridge`

Install the ROS 2 and Gazebo packages appropriate for your ROS 2 distribution
before building. Package names and compatible simulator versions can vary by
distribution.

## Build

From the workspace root, source your ROS 2 installation and install any
dependencies declared by the packages:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
source install/setup.bash
```

In each new terminal, source both the ROS 2 installation and this workspace
before running its launch files:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
source install/setup.bash
```

## Run

### View the robot in RViz

```bash
ros2 launch my_robot_description display.launch.py
```

This starts the robot state publisher, joint-state publisher GUI, and RViz with
the project's saved configuration. An equivalent XML launch file is also
available:

```bash
ros2 launch my_robot_description display.launch.xml
```

### Launch the Gazebo simulation

```bash
ros2 launch my_robot_bringup my_robot.launch.xml
```

The bringup launch file starts an empty Gazebo Sim world, publishes the robot
description, spawns the robot, starts the configured topic bridges, and opens
RViz.

## ROS–Gazebo topics

The bridge configuration in `my_robot_bringup/config/gazebo_bridge.yaml`
connects:

| ROS topic | Direction | Purpose |
| --- | --- | --- |
| `/cmd_vel` | ROS → Gazebo | Send velocity commands to the simulated differential drive. |
| `/joint_states` | Gazebo → ROS | Publish the simulated robot's joint states. |
| `/tf` | Gazebo → ROS | Publish the simulated robot's transforms. |

## Project scope

This repository focuses on robot description and basic simulation bringup. It
does not currently include a navigation stack, mapping, localization, or
perception pipeline. These can be added as future extensions to the platform.
