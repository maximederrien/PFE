cmake_minimum_required(VERSION 3.8)
project(bot_driver)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  #add_compile_options(-Wall -Wextra -Wpedantic)
  #add_compile_options( )
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(CppLinuxSerial REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(std_msgs REQUIRED)

# uncomment the following section in order to fill in

include_directories( src/uC_com/com_std )

add_executable(comUc 
	src/uC_com/uC_com.cpp
	src/uC_com/com_std/con_std.cpp
	src/uC_com/com_std/com_std.h	
	)

add_executable( cam_com
	src/cam_com/cam_com.cpp
	)

target_link_libraries(comUc CppLinuxSerial::CppLinuxSerial)
#ament_target_dependencies(cam_com OpenCV cv_bridge sensor_msgs)

ament_target_dependencies(cam_com OpenCV cv_bridge rclcpp sensor_msgs)
ament_target_dependencies(comUc rclcpp std_msgs)

install(TARGETS cam_com DESTINATION lib/${PROJECT_NAME})
install(TARGETS comUc DESTINATION lib/${PROJECT_NAME})
install(DIRECTORY launch DESTINATION share/${PROJECT_NAME}/)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
