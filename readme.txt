usage 

$1 roslaunch simple_example_description spawn_robot.launch

$2 rosrun rviz rviz -d `rospack find simple_example_description`/rviz/simple_urdf_example.rviz

$3 rostopic pub -1 /simple_model/base_to_second_joint_position_controller/command std_msgs/Float64 "data: 3"






Problem1
 rviz zeigt Robot nicht

Lösung
 $ export LC_NUMERIC="en_US.UTF-8"


Problem2
[ WARN] [1558685577.720293619]: The root link base_link has an inertia specified in the URDF, but KDL does not support a root link with an inertia.  As a workaround, you can add an extra dummy link to your URDF

Lösung Dummy-Link hinzufügen
 
        <?xml version="1.0"?>
        <robot name="simple_example">

        <link name="world" />
        <joint name="world_to_base_link=" type="fixed">
            <parent link="world"/>
            <child link="base_link"/>
        </joint>

        <link name="base_link">





Problem 3
[ERROR] [1558685591.082668751, 0.195000000]: No p gain specified for pid.  Namespace: /simple_model/gazebo_ros_control/pid_gains/base_to_second_joint

Lösung
config.ymal pid_gains ergänzen
simple_model:
    joint_state_controller:
        type: joint_state_controller/JointStateController
        publish_rate: 20
 
    base_to_second_joint_position_controller:
        type: velocity_controllers/JointVelocityController
        joint: base_to_second_joint
	
	gazebo_ros_control:
       pid_gains:
            base_to_second_joint: {p: 1.0, i: 1.0, d: 0.0}

====> Dann verschwindet der obere Teil des Robots !?!?!?


Problem 4
[Err] [REST.cc:205] Error in REST request

Lösung 
https://bitbucket.org/osrf/gazebo/issues/2607/error-restcc-205-during-startup-gazebo
You need to change ~/.ignition/fuel/config.yaml as following.

    url: https://api.ignitionfuel.org

to

    url: https://api.ignitionrobotics.org



#####  Los gehts 
rostopic pub -1 /simple_model/base_to_second_joint_position_controller/command std_msgs/Float64 3.0


####effort controller
simple_model:
    joint_state_controller:
        type: joint_state_controller/JointStateController   
        publish_rate: 20
 
    base_to_second_joint_position_controller:
        type: effort_controllers/JointPositionController   => type: velocity_controllers/JointVelocityController
        joint: base_to_second_joint
        pid: {p: 1.0, i: 1.0, d: 0.0}
        
        
     <joint name="base_to_second_joint">
      <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
      
      => <hardwareInterface>hardware_interface/VelocityJointInterface</hardwareInterface>
    </joint>


Problem 5:
Could not load controller 'base_to_second_joint_position_controller' because controller type 'velocity_controllers/JointVelocityController' does not exist.

=> 
sudo apt-get install ros-noetic-velocity-controllers
