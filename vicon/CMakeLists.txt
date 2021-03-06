cmake_minimum_required(VERSION 2.8.3)
project(vicon)

# set default build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

find_package(catkin REQUIRED COMPONENTS roscpp geometry_msgs nav_msgs std_srvs
  message_generation cmake_modules vicon_driver)
find_package(Eigen REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

add_message_files(DIRECTORY msg)
add_service_files(FILES SetPose.srv)
generate_messages(DEPENDENCIES nav_msgs geometry_msgs)

catkin_package(CATKIN_DEPENDS roscpp geometry_msgs nav_msgs std_srvs
  message_runtime cmake_modules vicon_driver
  DEPENDS Eigen Boost)

include_directories(${catkin_INCLUDE_DIRS} ${Eigen_INCLUDE_DIRS})

add_executable(vicon src/vicon.cpp)
target_link_libraries(vicon ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(vicon ${catkin_EXPORTED_TARGETS} vicon_generate_messages_cpp)

add_executable(vicon_calibrate src/vicon_calibrate.cpp)
target_link_libraries(vicon_calibrate ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(vicon_calibrate ${catkin_EXPORTED_TARGETS} vicon_generate_messages_cpp)

install(TARGETS vicon vicon_calibrate
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
