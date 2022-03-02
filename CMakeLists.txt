#
# (C) Copyright 2015-2022 Johns Hopkins University (JHU), All Rights Reserved.
#
# --- begin cisst license - do not edit ---
#
# This software is provided "as is" under an open source license, with
# no warranty.  The complete license can be found in license.txt and
# http://www.cisst.org/cisst/license.txt.
#
# --- end cisst license ---

cmake_minimum_required (VERSION 3.10)
project (optoforce VERSION 1.1)

find_package (ament_cmake REQUIRED)
find_package (rclcpp REQUIRED)
find_package (std_msgs REQUIRED)
find_package (geometry_msgs REQUIRED)
find_package (sensor_msgs REQUIRED)
find_package (diagnostic_msgs REQUIRED)
find_package (cisst_msgs REQUIRED)
find_package (tf2_ros REQUIRED)
find_package (tf2_msgs REQUIRED)
find_package (crtk_msgs REQUIRED)
find_package (cisst_ros2_bridge REQUIRED)
find_package (cisst_ros2_crtk REQUIRED)

# find cisst and make sure the required libraries have been compiled
set (REQUIRED_CISST_LIBRARIES
     cisstCommon
     cisstCommonQt
     cisstVector
     cisstVectorQt
     cisstOSAbstraction
     cisstMultiTask
     cisstMultiTaskQt
     cisstParameterTypes
     cisstParameterTypesQt
     cisstQt
     )

find_package (cisst 1.1.0 REQUIRED ${REQUIRED_CISST_LIBRARIES})

if (cisst_FOUND_AS_REQUIRED)

  include (${CISST_USE_FILE})

  find_package (sawOptoforceSensor 1.1.0 REQUIRED)

  include_directories (
    ${optoforce_SOURCE_DIR}
    ${CATKIN_DEVEL_PREFIX}/include
    ${sawOptoforceSensor_INCLUDE_DIR}
    )

  link_directories (
    ${sawOptoforceSensor_LIBRARY_DIR}
    )

  # executable
  add_executable (optoforce mainQt.cpp)
  target_link_libraries (
    optoforce
    ${sawOptoforceSensor_LIBRARIES}
    ${catkin_LIBRARIES}
    )
  cisst_target_link_libraries (optoforce ${REQUIRED_CISST_LIBRARIES})
  ament_target_dependencies (
    optoforce
    rclcpp
    std_msgs
    geometry_msgs
    sensor_msgs
    diagnostic_msgs
    cisst_msgs
    tf2_ros
    crtk_msgs
    cisst_ros2_bridge
    cisst_ros2_crtk
    )

  install (TARGETS optoforce
    LIBRARY DESTINATION lib/${PROJECT_NAME}
    RUNTIME DESTINATION lib/${PROJECT_NAME}
    LIBRARY DESTINATION lib
    INCLUDES DESTINATION include
    )

  ament_package ()

endif (cisst_FOUND_AS_REQUIRED)
