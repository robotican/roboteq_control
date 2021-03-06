cmake_minimum_required(VERSION 2.8.3)
project(roboteq_control)

## Add support for C++11, supported in ROS Kinetic and newer
add_definitions(-std=c++11)

## Find catkin macros and librariess
find_package(catkin REQUIRED
                COMPONENTS
                    controller_manager
                    hardware_interface
                    diagnostic_updater
                    roslaunch
                    serial
                    roscpp
                    sensor_msgs
                    std_srvs
                    std_msgs
                    joint_limits_interface
                    genmsg
                    urdf
)
## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED
                COMPONENTS
                    chrono
                    system
                    thread
)

## Generate messages in the 'msg' folder
add_message_files(
    FILES
    ControlStatus.msg
    MotorStatus.msg
    Peripheral.msg
)

## Generate services in the 'srv' folder
add_service_files(
   FILES
   Service.srv
)

## Generate added messages and services
generate_messages(DEPENDENCIES std_msgs)

###################################
## catkin specific configuration ##
###################################
catkin_package(
    INCLUDE_DIRS
        include
    CATKIN_DEPENDS
        diagnostic_updater
        std_msgs
        std_srvs
        hardware_interface
        roscpp
        sensor_msgs
        joint_limits_interface
    DEPENDS
        Boost
    LIBRARIES
        roboteq_control
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${Boost_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

## Declare a C++ library
add_library(${PROJECT_NAME}
        src/roboteq/roboteq.cpp include/roboteq/roboteq.h
        src/roboteq/serial_controller.cpp include/roboteq/serial_controller.h
        src/roboteq/roboteq.cpp
        src/roboteq/motor.cpp
        src/configurator/motor_param.cpp
        src/configurator/motor_pid.cpp
        src/configurator/gpio_analog.cpp
        src/configurator/gpio_pulse.cpp
        src/configurator/gpio_encoder.cpp)

###################################
#set(roboteq_control_SRC
#  src/roboteq_control.cpp
#  src/roboteq/serial_controller.cpp
#  src/roboteq/roboteq.cpp
  #src/roboteq/motor.cpp
  #src/configurator/motor_param.cpp
  #src/configurator/motor_pid.cpp
  #src/configurator/gpio_analog.cpp
  #src/configurator/gpio_pulse.cpp
  #src/configurator/gpio_encoder.cpp
#)

# Declare a cpp executable ##########################################################
#add_executable(${PROJECT_NAME}_node ${roboteq_control_SRC})
#add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS})
#target_link_libraries(${PROJECT_NAME}_node ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME driver_node PREFIX "")

add_dependencies(${PROJECT_NAME} ${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${${PROJECT_NAME}_gencpp} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME}
        ${catkin_LIBRARIES}
        )

## Declare a cpp executable #######################################################
#add_executable(roboteq_node ${roboteq_control_SRC})
#target_link_libraries(roboteq_node ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#add_dependencies(roboteq_node ${${PROJECT_NAME}_EXPORTED_TARGETS})

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

# Mark executables and/or libraries for installation
 install(TARGETS ${PROJECT_NAME}#_node
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
 )

## Mark executables and/or libraries for installation
install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )

# Mark cpp header files for installation
 install(DIRECTORY include/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
   PATTERN ".git" EXCLUDE
 )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_roboteq_control.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
