cmake_minimum_required(VERSION 2.8.3)
project(kvaser_interface)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  can_msgs
  roslint
)

catkin_package(
  CATKIN_DEPENDS roscpp can_msgs
  INCLUDE_DIRS include
  LIBRARIES ros_${PROJECT_NAME}
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(ros_${PROJECT_NAME}
  src/kvaser_interface.cpp
)

target_link_libraries(ros_${PROJECT_NAME}
  canlib
  ${catkin_LIBRARIES}
)

add_executable(kvaser_can_bridge
  src/kvaser_can_bridge.cpp
)

target_link_libraries(kvaser_can_bridge
  ros_${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

set(ROSLINT_CPP_OPTS "--filter=-runtime/threadsafe_fn,-build/namespaces,-build/c++11")
roslint_cpp()

install(
  TARGETS
  ros_${PROJECT_NAME}
  kvaser_can_bridge
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
