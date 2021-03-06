cmake_minimum_required(VERSION 3.9) # CMP0069 NEW

project(simdjson
  DESCRIPTION "Parsing gigabytes of JSON per second"
  LANGUAGES CXX C
)

set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 3)
set(PROJECT_VERSION_PATCH 1)
set(SIMDJSON_LIB_VERSION "0.3.1" CACHE STRING "simdjson library version")
set(SIMDJSON_LIB_SOVERSION "1" CACHE STRING "simdjson library soversion")
set(SIMDJSON_GITHUB_REPOSITORY https://github.com/simdjson/simdjson)

include(GNUInstallDirs)
include(simdjson-flags.cmake)
include(simdjson-user-cmakecache.cmake)

#
# Set up test data
#
enable_testing()
add_subdirectory(jsonchecker)
add_subdirectory(jsonexamples)
add_library(test-data INTERFACE)
target_link_libraries(test-data INTERFACE jsonchecker-data jsonexamples-data)

#
# Create the top level simdjson library (must be done at this level to use both src/ and include/
# directories) and tools
#
add_subdirectory(include)
add_subdirectory(src)
add_subdirectory(windows)
add_subdirectory(tools)
add_subdirectory(singleheader)

#
# Compile tools / tests / benchmarks
#

add_subdirectory(dependencies)
add_subdirectory(tests)
add_subdirectory(examples)
add_subdirectory(benchmark)
add_subdirectory(fuzz)

#
# CPack
#
set(CPACK_PACKAGE_VENDOR "Daniel Lemire")
set(CPACK_PACKAGE_CONTACT "lemire@gmail.com")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Parsing gigabytes of JSON per second")
set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")

set(CPACK_RPM_PACKAGE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")

include(CPack)
