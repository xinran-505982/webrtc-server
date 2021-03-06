cmake_minimum_required(VERSION 2.8)

project (mywebrtc)

function(test_lib LIB_NAME)
    if (${LIB_NAME} MATCHES "^.*-NOTFOUND")
        message(FATAL_ERROR "lib not found: " ${LIB_NAME} " check README")
        return()
    endif(${LIB_NAME} MATCHES "^.*-NOTFOUND")
endfunction(test_lib)

set(ERIZO_CMAKE_CXX_FLAGS "${ERIZO_CMAKE_CXX_FLAGS}   -DWEBRTC_POSIX")
set(ERIZO_CMAKE_CXX_FLAGS "${ERIZO_CMAKE_CXX_FLAGS}   -fPIC -DWEBRTC_LINUX")
set(ERIZO_CMAKE_CXX_FLAGS "${ERIZO_CMAKE_CXX_FLAGS} -DBOOST_THREAD_PROVIDES_FUTURE -DBOOST_THREAD_PROVIDES_FUTURE_CONTINUATION -DBOOST_THREAD_PROVIDES_FUTURE_WHEN_ALL_WHEN_ANY")
set(CMAKE_CXX_FLAGS "-g -Wall -std=c++11 ${ERIZO_CMAKE_CXX_FLAGS}")

message("------------ ${CMAKE_CURRENT_SOURCE_DIR}")

set(ToolKit_Include ${CMAKE_CURRENT_SOURCE_DIR}/ZLMediaKit/3rdpart/ZLToolKit/src)
set(MediaKit_Include ${CMAKE_CURRENT_SOURCE_DIR}/ZLMediaKit/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/webrtc/src)
include_directories(erizo)
include_directories(${ToolKit_Include})
include_directories(${MediaKit_Include})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(rtsp)

include_directories(thirdparty/include)
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/lib)

#boost
find_package(Boost REQUIRED COMPONENTS regex filesystem thread system)

# LOG4CXX
find_library(LOG log4cxx)
test_lib(${LOG})

#muduo_net
file(GLOB NET_SRC ${CMAKE_CURRENT_SOURCE_DIR}/net/*.cpp)
add_library(net STATIC ${NET_SRC})

add_subdirectory(erizo)
add_subdirectory(webrtc/src)
add_subdirectory(ZLMediaKit)
add_subdirectory(example)

