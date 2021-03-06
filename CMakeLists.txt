cmake_minimum_required(VERSION 2.8.3)
project(explo_octo_3d)

set(octomap_DIR "/opt/ros/indigo/share/octomap")
set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS} -O3")

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    message("OpenMP enabled")
endif()


## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  octomap_msgs
  octomap_ros
  octomap_server
  pcl_ros
  roscpp
  std_msgs
  move_base_msgs
  actionlib
  laser_geometry
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend and a run_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependencies might have been
##     pulled in transitively but can be declared for certainty nonetheless:
##     * add a build_depend tag for "message_generation"
##     * add a run_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
# generate_messages(
#   DEPENDENCIES
#   geometry_msgs#   octomap_msgs#   std_msgs
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES explo_octo_3d
#  CATKIN_DEPENDS geometry_msgs octomap_msgs octomap_ros octomap_server pcl_ros roscpp std_msgs actionlib move_base_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  $(OCTOMAP_INCLUDE_DIRS)
  include/
)

## Declare a cpp library
# add_library(explo_octo_3d
#   src/${PROJECT_NAME}/explo_octo_3d.cpp
# )

add_library(navigation_utils    src/navigation_utils.cpp)
# add_library(gpregressor         src/gpregressor.cpp)
# add_library(covMatern           src/covMaterniso3.cpp)

## Declare a cpp executable
# add_executable(explo_octo_3d_node src/explo_octo_3d_node.cpp)
add_executable(navigation           src/navigation_utils.cpp)
# add_executable(explo_sam_2d         src/explo_sam_2d.cpp )
add_executable(explo_sam_2d_new     src/explo_sam_2d_new.cpp )
# add_executable(explo_gp_2d          src/explo_gp_2d.cpp src/gpregressor.cpp  src/covMaterniso3.cpp)
add_executable(explo_gp_2d_new      src/explo_gp_2d_new.cpp src/gpregressor.cpp  src/covMaterniso3.cpp)
# add_executable(simu_explo           src/explo_octo_3d_node.cpp src/gpregressor.cpp  src/covMaterniso3.cpp)
# add_executable(simu_gazebo          src/simu_gazebo.cpp)
add_executable(save_to_ply          src/save_ply.cpp)
#add_executable(isam_ros             src/isam_example.cpp)
add_executable(scan_to_pcl          src/scan_to_pcl.cpp)
add_executable(explo_sam_2d_turtlebot     src/explo_sam_2d_turtlebot.cpp )


## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(explo_octo_3d_node explo_octo_3d_generate_messages_cpp)
# add_dependencies(navigation move_base_msgs actionlib roscpp)

## Specify libraries to link a library or executable target against
# target_link_libraries(explo_octo_3d_node
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(explo_sam_2d
#   ${catkin_LIBRARIES}
#   ${OCTOMAP_LIBRARIES}
#   navigation_utils
# )

target_link_libraries(explo_sam_2d_new
  ${catkin_LIBRARIES}
  ${OCTOMAP_LIBRARIES}
  navigation_utils
)

target_link_libraries(explo_sam_2d_turtlebot
  ${catkin_LIBRARIES}
  ${OCTOMAP_LIBRARIES}
  navigation_utils
)

target_link_libraries(explo_gp_2d_new
  ${catkin_LIBRARIES}
  ${OCTOMAP_LIBRARIES}
  navigation_utils
)


# target_link_libraries(simu_gazebo
#   ${catkin_LIBRARIES}
#   ${OCTOMAP_LIBRARIES}
#   navigation_utils
# )

# target_link_libraries(gp_test
#   ${catkin_LIBRARIES}
#   # covMatern
# )

target_link_libraries(navigation
  ${catkin_LIBRARIES}
)

target_link_libraries(save_to_ply
  ${catkin_LIBRARIES}
)

target_link_libraries(scan_to_pcl
  ${catkin_LIBRARIES}
)

#target_link_libraries(isam_ros
 # ${catkin_LIBRARIES} isam cholmod
#)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS explo_octo_3d explo_octo_3d_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_explo_octo_3d.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

# add_dependencies(explo_sam_2d explo_octo_3d_gemcpp)
