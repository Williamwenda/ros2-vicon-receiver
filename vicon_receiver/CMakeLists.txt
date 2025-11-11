cmake_minimum_required(VERSION 3.5)
project(vicon_receiver)

# C99 / C++14
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -g)
endif()

include_directories(
  include
  ${Boost_INCLUDE_DIRS}
)

# deps
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(rclcpp REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)
find_package(tf2_ros REQUIRED)
find_package(rosidl_typesupport_cpp REQUIRED)

# interfaces
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/Position.msg"
)

ament_export_dependencies(rosidl_default_runtime)

# node target
add_executable(vicon_client
  src/vicon_receiver/communicator.cpp
  src/vicon_receiver/publisher.cpp
)
target_link_libraries(vicon_client ViconDataStreamSDK_CPP ${Boost_LIBRARIES})
ament_target_dependencies(vicon_client rclcpp tf2_ros)

# NEW: link the generated C++ typesupport
rosidl_get_typesupport_target(cpp_typesupport_target
  "${PROJECT_NAME}" "rosidl_typesupport_cpp")
target_link_libraries(vicon_client "${cpp_typesupport_target}")

# install
install(TARGETS vicon_client DESTINATION lib/${PROJECT_NAME})

# launch files
install(DIRECTORY launch DESTINATION share/${PROJECT_NAME}/)

ament_package()