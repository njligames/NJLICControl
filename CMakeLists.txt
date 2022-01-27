cmake_minimum_required(VERSION 3.17)
project(NJLICControl)

set(CMAKE_CXX_STANDARD 14)

# The following variables are used to build the targets.
# ${PROJECT_NAME}_THIRDPARTY_INCLUDE_DIRS
# ${PROJECT_NAME}_PUBLIC_DIRECTORIES
# ${PROJECT_NAME}_PRIVATE_DIRECTORIES
# ${PROJECT_NAME}_TARGET_LIBRARIES
# ${PROJECT_NAME}_DEFINITIONS
# ${PROJECT_NAME}_SRCS
# ${PROJECT_NAME}_HDRS
# ${PROJECT_NAME}_SRCS_TEST
# ${PROJECT_NAME}_HDRS_TEST

include(cmake/configure.cmake)

configure_file(cmake/njlicrenderer.cmake.in njlicrenderer-download/CMakeLists.txt)
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" . WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/njlicrenderer-download )
execute_process(COMMAND ${CMAKE_COMMAND} --build . WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/njlicrenderer-download )
add_subdirectory(${CMAKE_BINARY_DIR}/njlicrenderer-src ${CMAKE_BINARY_DIR}/njlicrenderer-build)

set(NJLICRENDERER_INCLUDE_DIR ${CMAKE_BINARY_DIR}/njlicrenderer-src/include)
set(GLM_INCLUDE_DIR ${CMAKE_BINARY_DIR}/njlicrenderer-build/glm-src)
set(SDL_INCLUDE_DIR ${CMAKE_BINARY_DIR}/njlicrenderer-build/sdl-src/include)
set(NLOHMANN_INCLUDE_DIR ${CMAKE_BINARY_DIR}/njlicrenderer-build/nlohmann_json-src/include)

list(APPEND ${PROJECT_NAME}_THIRDPARTY_INCLUDE_DIRS
        ${NJLICRENDERER_INCLUDE_DIR}
        ${GLM_INCLUDE_DIR}
        ${SDL_INCLUDE_DIR}
        ${NLOHMANN_INCLUDE_DIR}
        )

list(APPEND ${PROJECT_NAME}_THIRDPARTY_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/include/thirdparty)

list(APPEND ${PROJECT_NAME}_TARGET_LIBRARIES
        NJLICRenderer-static
        )

list(APPEND ${PROJECT_NAME}_PUBLIC_DIRECTORIES
        ${CMAKE_CURRENT_SOURCE_DIR}/platform/macos
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${${PROJECT_NAME}_THIRDPARTY_INCLUDE_DIRS}
        )

list(APPEND ${PROJECT_NAME}_PRIVATE_DIRECTORIES
        ${CMAKE_CURRENT_SOURCE_DIR}
        )

list(APPEND ${PROJECT_NAME}_DEFINITIONS __GL_2__=1)

list(APPEND ${PROJECT_NAME}_SRCS
        # ${CMAKE_CURRENT_SOURCE_DIR}/src/BitmapFont.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/Color.cpp
        # ${CMAKE_CURRENT_SOURCE_DIR}/src/Node.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/PubSub.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/Scene.cpp
        )

list(APPEND ${PROJECT_NAME}_HDRS
        # ${CMAKE_CURRENT_SOURCE_DIR}/include/BitmapFont.h
        ${CMAKE_CURRENT_SOURCE_DIR}/include/Color.h
        # ${CMAKE_CURRENT_SOURCE_DIR}/include/Node.h
        ${CMAKE_CURRENT_SOURCE_DIR}/include/PubSub.h
        ${CMAKE_CURRENT_SOURCE_DIR}/include/Scene.h
        )

list(APPEND ${PROJECT_NAME}_SRCS_TEST ${CMAKE_CURRENT_SOURCE_DIR}/test/TestRender.cpp)
list(APPEND ${PROJECT_NAME}_HDRS_TEST ${CMAKE_CURRENT_SOURCE_DIR}/test/TestRender.h)
list(APPEND ${PROJECT_NAME}_SRCS_TEST ${CMAKE_CURRENT_SOURCE_DIR}/test/TestColor.cpp)
list(APPEND ${PROJECT_NAME}_HDRS_TEST ${CMAKE_CURRENT_SOURCE_DIR}/test/TestColor.h)

# # Download and unpack googletest at configure time
# configure_file(cmake/googletest.cmake.in
#         googletest-download/CMakeLists.txt)
# execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
#         WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
# execute_process(COMMAND ${CMAKE_COMMAND} --build .
#         WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
#
# # Prevent GoogleTest from overriding our compiler/linker options
# # when building with Visual Studio
# set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
#
# # Add googletest directly to our build. This adds
# # the following targets: gtest, gtest_main, gmock
# # and gmock_main
# add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/googletest-src
#         ${CMAKE_CURRENT_BINARY_DIR}/googletest-build)
#
# # The gtest/gmock targets carry header search path
# # dependencies automatically when using CMake 2.8.11 or
# # later. Otherwise we have to add them here ourselves.
# if (CMAKE_VERSION VERSION_LESS 2.8.11)
#     include_directories("${gtest_SOURCE_DIR}/include"
#             "${gmock_SOURCE_DIR}/include")
# endif()

add_subdirectory(${PLATFORM_DIR})
