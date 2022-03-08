cmake_minimum_required(VERSION 3.8)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

project(ublox_linux
  LANGUAGES CXX
  VERSION 2.0.5)

set(SPARKFUN_SRC SparkFun_u-blox_GNSS_Arduino_Library)

add_library(ublox_linux SHARED
  SparkFun_u-blox_GNSS_Arduino_Library/src/SparkFun_u-blox_GNSS_Arduino_Library.cpp
  src/Print.cpp
  src/Utils.cpp)

target_include_directories(ublox_linux PUBLIC
  $<INSTALL_INTERFACE:include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/inc>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SparkFun_u-blox_GNSS_Arduino_Library/src>)

set_property(TARGET ublox_linux PROPERTY CXX_STANDARD 11)
target_compile_options(ublox_linux PUBLIC -Wno-deprecated -fconcepts -Wall)
target_compile_definitions(ublox_linux PUBLIC LINUX_PLATFORM)

if (DEFINED PROJECT_NAME)
  install(TARGETS ublox_linux
    EXPORT ublox-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
  install(DIRECTORY inc/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ublox_linux)
  install(DIRECTORY SparkFun_u-blox_GNSS_Arduino_Library/src/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ublox_linux)
  install(EXPORT ublox-targets
    FILE UbloxTargets.cmake
    NAMESPACE ublox::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ublox_linux)

  configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/UbloxLinuxConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/UbloxLinuxConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ublox_linux)

  install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/UbloxLinuxConfig.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ublox_linux)
endif()
