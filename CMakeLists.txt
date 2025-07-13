
cmake_minimum_required(VERSION 3.16)
project(metal-cpp VERSION 15.2)
set(CMAKE_CXX_STANDARD 20)

add_library(metal-cpp INTERFACE)

include(GNUInstallDirs)
target_include_directories(metal-cpp INTERFACE
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

install(TARGETS metal-cpp
  EXPORT metal-cpp-targets)

install(EXPORT metal-cpp-targets
  FILE metal-cpp-targets.cmake
  NAMESPACE metal::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/metal-cpp)
  
# Create a config file for the package
include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/metal-cpp-config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/metal-cpp-config.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/metal-cpp)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/metal-cpp-config.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/metal-cpp)

# Install from current directory 'Metal' into include/Metal
set(FRAMEWORK_INCLUDE_DIRS
    Metal
    MetalFX
    QuartzCore
    Foundation
)

foreach(_hdr_dir IN LISTS FRAMEWORK_INCLUDE_DIRS)
    install(
        DIRECTORY       ${CMAKE_CURRENT_SOURCE_DIR}/${_hdr_dir}/
        DESTINATION     ${CMAKE_INSTALL_INCLUDEDIR}/${_hdr_dir}
        FILES_MATCHING  PATTERN "*.h" PATTERN "*.hpp" PATTERN "*.inl"
                        PATTERN "*.hh" PATTERN ".DS_Store" EXCLUDE
    )
endforeach()

include(CPack)
