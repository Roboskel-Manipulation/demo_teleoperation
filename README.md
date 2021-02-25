# Teleoperation of UR3 Cobot

## Description
This package launches the necessary nodes for the teleoperation of a UR3 cobot.

<img src="https://github.com/Roboskel-Manipulation/demo_teleoperation/blob/main/pipeline-git.png" />

### Human Position Tracking
The teleoperation is based on visual input. An RGB-D camera is used for human tracking. [Openpose](https://github.com/CMU-Perceptual-Computing-Lab/openpose) is used for recognising 2D human joint positions. The 3D position of the wrist is then acquired via the associated point cloud information. The 3D  coordinates are expressed in a static reference frame and then used as potential trajectory points for the control of the robot's end-effector (EE) position. This part of the pipeline is implemented in [openpose_3D_localization](https://github.com/Roboskel-Manipulation/openpose_3D_localization).

### Robot Trajectory Generation
Once the human movement onset is detected, every human (right) wrist 3D position that becomes available is first checked to avoid outliers (points that diverge over 10cm from the previous one). Filtered 3D human positions are translated to UR3 EE positions considering the relative position between the human wrist and the EE effector at the beginning of a teleoperation cycle. The translated 3D EE positions are checked in terms of robot limits to ensure that they do not lead to self-collision or over-extension of the robot arm.
Valid EE positions are considered as desired positions and used for the calculation of the robot commanded velocities (see below).  This part of the pipeline is implemented in the [trajectory_process_utils](https://github.com/Roboskel-Manipulation/trajectory_process_utils) package.
 
A teleoperation cycle can be suspended by lifting the left arm above the left shoulder ([keypoints_relative_pos](https://github.com/Roboskel-Manipulation/keypoints_relative_pos)).  If the operator wishes to change his operating zone, he can recalibrate the relative position between the EE and the human operator by moving his wrist to a new starting point and staying still for approximately 3 seconds. After that he can lower his other arm and the new teleoperation cycle starts. 
 
### Robot Motion Generation
 
The desired robot positions are used as input to the [cartesian_trajectory_tracking](https://github.com/Roboskel-Manipulation/cartesian_trajectory_tracking) package which generates the commanded EE velocities published to the [CVC](https://github.com/Roboskel-Manipulation/manos/tree/updated_driver/manos_cartesian_control). Note that the user needs to choose the control law (P or PD controller) for the generation of the commanded velocities. The output of the CVC is fed to the UR3 robot driver.

### Pipeline demonstrations 
An interactive [3D plot](https://htmlpreview.github.io/?https://github.com/Roboskel-Manipulation/demo_teleoperation/blob/main/3D_visualization.html) shows the human movement and the respective robot motions using the two controllers during a 3D movement.  
A real-time demonstration comparing the two controllers and other real-time demonstrations of the performance of the teleoperation with the PD-controller are available at [Vimeo](https://vimeo.com/showcase/7718151/).

## Run
`roslaunch openpose_teleoperation reactive_framework.launch`

## Arguments
Input modality
* visual_input: True if using visual input to produce the 3D keypoints either using the real camera or a rosbag. False if using already obtained 3D keypoints.
* sim: True if using visual input from a rosbag.
* live_camera: True if frames are generated by an RGB-D camera (False if they are generated by rosbags)
NOTE: sim and live_camera arguments need to be set only if visual_input is set to true.

Robot
* halt_motion: True for enabling the user to halt the robot motion by bringing his left wrist higher than his left shoulder
* p_control: True for P controller (False for PD controller)
* gazebo: True if using gazebo
