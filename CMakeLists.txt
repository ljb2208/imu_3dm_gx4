cmake_minimum_required(VERSION 2.8.3)
project(imu_3dm_gx4)
find_package(catkin REQUIRED COMPONENTS
  diagnostic_updater
  message_generation
  roscpp
  geometry_msgs
  sensor_msgs
)

add_message_files(DIRECTORY msg)
generate_messages(DEPENDENCIES geometry_msgs)

catkin_package(
  CATKIN_DEPENDS message_runtime geometry_msgs sensor_msgs)

# include boost
find_package(Boost REQUIRED)
include_directories(${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIR})

add_definitions("-std=c++0x -Wall -Werror")

add_executable(${PROJECT_NAME} src/imu_3dm_gx4.cpp src/imu.cpp)
target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

add_dependencies(${PROJECT_NAME}
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
   FILES_MATCHING PATTERN "*.launch" PATTERN "*.machine" PATTERN "*.yaml" PATTERN "*.urdf"
)
