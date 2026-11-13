cmake_minimum_required(VERSION 3.24)
project(chat-server)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 只在没有指定构建类型时默认用 Release
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

# uWebSockets 的根目录
set(UWS_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/third_party/uWebSockets")

# 检查 uWebSockets 源码是否存在
if(NOT EXISTS "${UWS_ROOT}/src/App.h")
    message(FATAL_ERROR "uWebSockets not found at ${UWS_ROOT}. Run: git submodule update --init --recursive")
endif()

# 检查 uSockets.a 是否已经编译出来
if(NOT EXISTS "${UWS_ROOT}/uSockets/uSockets.a")
    message(FATAL_ERROR "uSockets.a not found. Run: cd ${UWS_ROOT} && make")
endif()

include_directories(${UWS_ROOT}/src)
include_directories(${UWS_ROOT}/uSockets/src)

add_executable(chat-server main.cpp)
target_link_libraries(chat-server ${UWS_ROOT}/uSockets/uSockets.a)