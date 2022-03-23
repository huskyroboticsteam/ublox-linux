cmake_minimum_required(VERSION 3.8)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

project(UbloxLinux
  LANGUAGES CXX
  VERSION 2.0.6.1)

set(SPARKFUN_SRC SparkFun_u-blox_GNSS_Arduino_Library)

add_library(ublox_linux SHARED
  SparkFun_u-blox_GNSS_Arduino_Library/src/SparkFun_u-blox_GNSS_Arduino_Library.cpp
  src/Print.cpp
  src/Utils.cpp)

target_include_directories(ublox_linux PUBLIC
  $<INSTALL_INTERFACE:include>
  $<INSTALL_INTERFACE:include/ublox_linux>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/inc>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SparkFun_u-blox_GNSS_Arduino_Library/src>)

target_link_libraries(ublox_linux util)

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
    FILE UbloxLinuxTargets.cmake
    NAMESPACE UbloxLinux::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/UbloxLinux)

  configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/UbloxLinuxConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/UbloxLinuxConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/UbloxLinux)

  install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/UbloxLinuxConfig.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/UbloxLinux)

  # CPack config
  set(CPACK_GENERATOR "DEB")
  set(CPACK_PACKAGE_NAME "ublox-linux")
  # Version format: major.minor.patch-tweak
  set(CPACK_PACKAGE_VERSION "${CMAKE_PROJECT_VERSION_MAJOR}.${CMAKE_PROJECT_VERSION_MINOR}.${CMAKE_PROJECT_VERSION_PATCH}-${CMAKE_PROJECT_VERSION_TWEAK}")
  set(CPACK_PACKAGE_FILE_NAME ${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION})
  set(CPACK_PACKAGE_CONTACT "Husky Robotics Team <uwrobots@uw.edu>")
  set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
  set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
  set(CPACK_PACKAGE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

  install(FILES ${CPACK_RESOURCE_FILE_README} ${CPACK_RESOURCE_FILE_LICENSE}
    DESTINATION share/docs/${PROJECT_NAME})

  include(CPack)
endif()
