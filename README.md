# simple_example_description
Working Version of https://www.theconstructsim.com/ros-qa-070-moving-joints-gazebo-simple-example

##Installation


  >$ cd catkin_ws/src
  
  >$ git clone  https://github.com/ProfJust/simple_example_description.git
  
  >$ cd catkin_ws
  
  >$ catkin_make

##Usage

  $1 roslaunch simple_example_description spawn_robot.launch

  $2 rosrun rviz rviz -d `rospack find simple_example_description`/rviz/simple_urdf_example.rviz

  $3 rostopic pub -1 /simple_model/base_to_second_joint_position_controller/command std_msgs/Float64 "data: 3"
