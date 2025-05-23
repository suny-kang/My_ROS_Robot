cmake_minimum_required(VERSION 3.5)
project(robot)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(builtin_interfaces REQUIRED)
# find_package(message_runtime REQUIRED)
# find_package(dynamic_reconfigure REQUIRED)

find_package(Boost REQUIRED COMPONENTS system filesystem thread)

include_directories(${Boost_INCLUDE_DIRS})

add_executable(robot 
    src/robot.cpp 
    src/serial_linux.cpp
)

target_link_libraries(robot ${Boost_LIBRARIES})

ament_target_dependencies(robot
    rclcpp
    rclpy
    geometry_msgs
    sensor_msgs
    std_msgs
    nav_msgs
    tf2
    tf2_ros
    builtin_interfaces
    # message_runtime
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

install(TARGETS
    robot
    DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY launch urdf meshes config
  DESTINATION share/${PROJECT_NAME}/
)

ament_package()
