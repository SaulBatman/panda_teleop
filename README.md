# change /opt/ros/noetic/share/moveit_servo setting
move launch/panda_servo.launch to /opt/ros/noetic/share/moveit_servo/launch/panda_servo.launch
move config/panda_servo_config.yaml to /opt/ros/noetic/share/moveit_servo/config/panda_servo_config.yaml

# change franka control setting
move config/default_controllers into /your_catkinws/franka_control/config

# change panda_moveit_config setting
move launch/franka_control.launch into /your_catkinws/panda_moveit_config/launch
move launch/ros_controllers.launch into /your_catkinws/panda_moveit_config/launch

# how to use
```
1. open franka control
roslaunch panda_moveit_config franka_control.launch robot_ip:=192.168.131.40 load_gripper:=false

2. switch controller
rosservice call /controller_manager/switch_controller "start_controllers: ['velocity_joint_trajectory_controller']
stop_controllers: ['franka_state_controller']
strictness: 0
start_asap: false
timeout: 0.0"

3. check it out
rosservice call /controller_manager/list_controllers

4. open moveit_servo
roslaunch moveit_servo panda_servo.launch

5. test it out
rostopic pub -r 100 -s /panda_servo/delta_twist_cmds geometry_msgs/TwistStamped "header: auto
twist:
  linear:
    x: 0.0
    y: 0.0  
    z: 0.005
  angular:
    x: 0.0  
    y: 0.0
    z: 0.0"
```

