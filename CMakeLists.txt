cmake_minimum_required(VERSION 3.5)
project(motion_tools)

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
#find_package(ament_cmake_python REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(visualization_msgs REQUIRED)


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

include_directories(
  include
)
add_executable(path_eval_node src/path_eval_node.cpp)
ament_target_dependencies(path_eval_node rclcpp geometry_msgs nav_msgs visualization_msgs)

install(
  TARGETS path_eval_node
  DESTINATION lib/${PROJECT_NAME}
)

ament_export_include_directories(include)

ament_python_install_package(${PROJECT_NAME})

# Install Python executables
install(PROGRAMS
 src/trajectory_player.py
 DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}/
)
install(DIRECTORY
  config
  DESTINATION share/${PROJECT_NAME}/
)

ament_package()
