cmake_minimum_required(VERSION 2.8.3)
project(arvlidar_ros)

set(ARVLIDAR_SDK_PATH "./sdk/")

FILE(GLOB ARVLIDAR_SDK_SRC 
  "${ARVLIDAR_SDK_PATH}/src/arch/linux/*.cpp"
  "${ARVLIDAR_SDK_PATH}/src/hal/*.cpp"
  "${ARVLIDAR_SDK_PATH}/src/*.cpp"
)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rosconsole
  sensor_msgs
)

include_directories(
  ${ARVLIDAR_SDK_PATH}/include
  ${ARVLIDAR_SDK_PATH}/src
  ${catkin_INCLUDE_DIRS}
)

catkin_package()

add_executable(arvlidarNode src/node.cpp ${ARVLIDAR_SDK_SRC})
target_link_libraries(arvlidarNode ${catkin_LIBRARIES})

add_executable(arvlidarNodeClient src/client.cpp)
target_link_libraries(arvlidarNodeClient ${catkin_LIBRARIES})

install(TARGETS arvlidarNode arvlidarNodeClient
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY rviz sdk
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  USE_SOURCE_PERMISSIONS
)


install(
  DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
