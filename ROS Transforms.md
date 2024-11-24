Transforms allow us to move between frames of reference in the real world.

Nodes create a transform my *broadcasting* it
Nodes utilize a transform by *listening* to it
Every transform has a single parent

**Static Transform**
> Does not actively change, assumed valid until a new transform is broadcast

**Dynamic Transform**
> Changes constantly. Subscribing nodes can throw an error if they don't get fresh data soon enough (due to the potential for danger if a transform is wrong)

![[Pasted image 20231007223106.png|500]]
# Static Transform
To create a static transform use:

```c
ros2 run tf2_ros static_transform_publisher <x> <y> <z> <yaw> <pitch> <roll> <parent_frame> <child_frame> 
```
**Note:** Yaw pitch and roll are in radians
### Example
Replicate the following transforms in ros:

![[Pasted image 20231007223655.png|300]]
**A:** Transform of robot 1 relative to origin

![[Pasted image 20231007223748.png|300]]
**B:** Transform of robot 2 relative to robot 1

**Run to create the transforms**
-> Use separate terminals

```c
ros2 run tf2_ros static_transform_publisher 2 1 0 0.785 0 0 world robot1
```

```C
ros2 run tf2_ros static_transform_publisher 1 0 0 0 0 0 robot1 robot2
```

**Visualize with RVIZ**
1) `$ ros2 run rviz2 rviz2`
2) Switch fixed frame to the target frame (Eg. *world*)
3) Hit 'ADD' at the bottom and add the tf visualizer
# Dynamic Transform
When working with parts that move, dynamic transforms are necessary. Dynamic transforms take joint states and use the joint states to recalculate the transforms on the fly.
### Example
Build the transforms of the robo boi
![[Pasted image 20231007231830.png|500]]

This is done with the *robot_state_publisher*. It takes in the current joint states as well as a representation of the robot as a URDF file and spits out all the transforms on `/tf_static` and `/tf` as well as a copy of the URDF on the `/robot_description` topic

![[Pasted image 20231007232732.png|500]]
For the joint states, normally they come from sensors, but for now, fake it with the *joint_state_publisher_gui*. It will read the URDF and then create sliders for all the joint states, publishing them on the `/joint_states` topic

**Start the publisher**
```C
ros2 run robot_state_publisher robot_state_publisher --ros-args -p robot_description:="$(xacro ~/example_urdf/example_robot.urdf.xacro)"
```
Note the weird use of xacro, this is because robot_description parameter expects the entire URDF, not just a file path, xacro unpacks it for us.

**Start the joint state gui**
```C
ros2 run joint_state_publisher_gui joint_state_publisher_gui
```

**Run rviz2 to visualize**
1) `$ rviz2`
2) Select fixed frame
3) Add TF to visualization
4) Add RobotModel to visualization
5) Change the description topic for RobotModel to the real one

**Debugging with view_frames.py**
1) `$ run tf2_tools view_frames.py`
2) Dumps a graph of the transforms in a PDF wherever the terminal is currently pointed