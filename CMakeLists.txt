cmake_minimum_required(VERSION 3.5.1)
project(yolotrt-sort)
add_executable(yolotrt-sort src/sample_detector.cpp)
set(CMAKE_CXX_COMPILIER "/usr/bin/g++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wno-write-strings")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath -Wl,$ORIGIN")

# Try to find Boost
find_package(Boost COMPONENTS program_options REQUIRED)
if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
else ()
    message(FATAL_ERROR "Could not locate Boost")
endif()
find_package(OpenCV REQUIRED)
if(Opencv_FOUND)
    message("found opencv")
endif()

add_library(sort SHARED
        src/kalman_filter.cpp
        src/munkres.cpp
        src/tracker.cpp
)
include_directories(${PROJECT_SOURCE_DIR}/include ${OpenCV_INCLUDE_DIRS})

target_link_libraries(yolotrt-sort ${PROJECT_SOURCE_DIR}/lib/libdetector.so)
target_link_libraries(yolotrt-sort sort ${OpenCV_LIBS} ${Boost_LIBRARIES})