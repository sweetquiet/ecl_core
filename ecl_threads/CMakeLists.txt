##############################################################################
# Cmake
##############################################################################

cmake_minimum_required(VERSION 3.5)
project(ecl_threads)

##############################################################################
# Thread Detection
##############################################################################

find_package(ament_cmake_ros REQUIRED)
find_package(ecl_build REQUIRED)
find_package(ecl_config REQUIRED)
find_package(ecl_errors REQUIRED)
find_package(ecl_concepts REQUIRED)
find_package(ecl_exceptions REQUIRED)
find_package(ecl_time REQUIRED)
find_package(ecl_utilities REQUIRED)

##############################################################################
# Project Configuration
##############################################################################

ament_package_xml()
ecl_enable_cxx14_compiler()
ecl_detect_threads()

if(ECL_PLATFORM_HAS_POSIX_THREADS)
  # TODO: move this over into ecl_detect_threads
  set(THREADS_PREFER_PTHREAD_FLAG ON)
  find_package(Threads REQUIRED)
  set (thread_INTERFACE "Threads::Threads" CACHE STRING "Platform thread interface")
else()
  set (thread_INTERFACE "" CACHE STRING "Platform thread interface")
endif()

##############################################################################
# Sources
##############################################################################

add_subdirectory(include)
add_subdirectory(src)

##############################################################################
# Exports
##############################################################################

ament_export_interfaces(${PROJECT_NAME})
ament_export_dependencies(
    ecl_config
    ecl_errors
    ecl_concepts
    ecl_exceptions
    ecl_time
    ecl_utilities
)
ament_export_libraries(${PROJECT_NAME})
ament_package(CONFIG_EXTRAS "${PROJECT_NAME}-extras.cmake")
