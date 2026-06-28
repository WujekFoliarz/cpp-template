cmake_minimum_required (VERSION 3.18)
set(CMAKE_CXX_STANDARD 23)
project(cpp-template)

file(GLOB_RECURSE MY_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

#add_library(${PROJECT_NAME} SHARED ${MY_SOURCES})
add_executable(${PROJECT_NAME} ${MY_SOURCES})

target_precompile_headers(${PROJECT_NAME} PRIVATE src/pch.hpp)
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src")

# Libraries

# add_subdirectory("deps/minhook")
# target_link_libraries(${PROJECT_NAME} PRIVATE )

# Extension

# set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX ".asi")
