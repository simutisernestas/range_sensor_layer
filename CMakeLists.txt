cmake_minimum_required(VERSION 3.5)
project(range_sensor_layer)

# clion can't find RMW without these
set(FastRTPS_INCLUDE_DIR /opt/ros/foxy/include)
set(FastRTPS_LIBRARY_RELEASE /opt/ros/foxy/lib/libfastrtps.so)

# Default to C99
if (NOT CMAKE_C_STANDARD)
    set(CMAKE_C_STANDARD 99)
endif ()

# Default to C++14
if (NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 14)
endif ()

if (CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wall -Wextra -Wpedantic)
endif ()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(angles REQUIRED)
find_package(nav2_costmap_2d REQUIRED)
find_package(dynamic_reconfigure REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(pluginlib REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)

include_directories(include)

set(dependencies
        rclcpp
        angles
        nav2_costmap_2d
        dynamic_reconfigure
        geometry_msgs
        pluginlib
        sensor_msgs
        tf2_geometry_msgs)


add_library(${PROJECT_NAME} src/range_sensor_layer.cpp)

ament_target_dependencies(${PROJECT_NAME} ${dependencies})

install(TARGETS ${PROJECT_NAME}
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)

install(FILES costmap_plugins.xml
        DESTINATION share/)

install(DIRECTORY include/
        DESTINATION include/)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

if (BUILD_TESTING)
    find_package(ament_lint_auto REQUIRED)
    ament_lint_auto_find_test_dependencies()
endif ()

ament_package()