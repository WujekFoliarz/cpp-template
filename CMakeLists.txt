cmake_minimum_required (VERSION 3.18)
set(CMAKE_CXX_STANDARD 23)
project(cpp-template)

file(GLOB_RECURSE MY_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

#add_library(${PROJECT_NAME} SHARED ${MY_SOURCES})
add_executable(${PROJECT_NAME} ${MY_SOURCES})

target_precompile_headers(${PROJECT_NAME} PRIVATE src/pch.hpp)
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src")

# -- Libraries --
# add_subdirectory("deps/raylib")
# target_link_libraries(${PROJECT_NAME} PRIVATE raylib)

# -- Extension --
# set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX ".asi")

# -- Strip debug symbols and statically link libgcc on mingw --
set(DEBUG_FILE "${PROJECT_NAME}.debug")

if(MINGW)
    set(DEBUG_FILE "${PROJECT_NAME}.pdb")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static -static-libgcc -static-libstdc++")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -static -static-libgcc -static-libstdc++")
endif()

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} --only-keep-debug $<TARGET_FILE:${PROJECT_NAME}> ${DEBUG_FILE}
    COMMAND ${CMAKE_OBJCOPY} --strip-all --strip-unneeded $<TARGET_FILE:${PROJECT_NAME}>
    COMMAND ${CMAKE_OBJCOPY} --add-gnu-debuglink=${DEBUG_FILE} $<TARGET_FILE:${PROJECT_NAME}>
)
