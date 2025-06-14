add_library(motor_driver SHARED motor_driver.cpp)

target_include_directories(motor_driver
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

# Reference: See libserial example project for integration with CMake
target_include_directories(motor_driver PUBLIC ${SERIAL_INCLUDE_DIRS})
target_link_libraries(motor_driver PUBLIC ${SERIAL_LDFLAGS} ${CMAKE_THREAD_LIBS_INIT})


add_library(diffdrive_mobot SHARED diffdrive_mobot.cpp wheel.cpp)

target_include_directories(diffdrive_mobot
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

ament_target_dependencies(diffdrive_mobot
  PUBLIC
    hardware_interface
    pluginlib
    rclcpp
)

target_link_libraries(diffdrive_mobot PUBLIC motor_driver)

install(
  TARGETS diffdrive_mobot motor_driver
  EXPORT ${PROJECT_NAME}-targets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_export_libraries(diffdrive_mobot)
ament_export_targets(${PROJECT_NAME}-targets HAS_LIBRARY_TARGET)
ament_export_dependencies(hardware_interface pluginlib rclcpp)
