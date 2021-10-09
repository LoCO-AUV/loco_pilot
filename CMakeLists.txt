cmake_minimum_required(VERSION 2.8.3)
project(loco_pilot)

find_package(catkin REQUIRED COMPONENTS
  dynamic_reconfigure
  rospy
  tf
  std_msgs
  sensor_msgs
  geometry_msgs
  mavros_msgs
  message_generation
  message_runtime
)

find_package(Bullet)
include_directories(${BULLET_INCLUDE_DIRS})

# Generate messages in the 'msg' folder
add_message_files(
  FILES
  AutopilotModes.msg
  Command.msg
  UberpilotStatus.msg
)

add_service_files(
  FILES
  SetAutopilotMode.srv
  SetAutopilotParams.srv
)

## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)

## Generate dynamic reconfigure parameters in the 'cfg' folder
generate_dynamic_reconfigure_options(
  cfg/Autopilot.cfg
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES mini_pilot
#  CATKIN_DEPENDS dynamic_reconfigure geometry_msgs rospy sensor_msgs tf
 DEPENDS message_runtime std_msgs geometry_msgs bullet geometry_msgs tf
)

###########
## Build ##
###########
include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)

add_executable(local_autopilot src/local_autopilot.cpp)
add_dependencies(local_autopilot ${${PROJECT_NAME}_EXPORTED_TARGETS} 
                                      ${PROJECT_NAME}_generate_messages_cpp)
target_link_libraries(local_autopilot ${catkin_LIBRARIES})