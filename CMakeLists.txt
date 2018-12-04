cmake_minimum_required(VERSION 3.12)
project(pugixml)

ADD_DEFINITIONS(-DPUGI_EXPORT)

set(CMAKE_CXX_STANDARD 14)

set(INC_DIR ./ include/pugixml)
set(LINK_DIR libs/${CMAKE_BUILD_TYPE})
set(SOURCE_FILES pugixml.cpp)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../libs/${CMAKE_BUILD_TYPE})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ../libs/${CMAKE_BUILD_TYPE})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../bin/${CMAKE_BUILD_TYPE})

include_directories(${INC_DIR})
link_directories(${LINK_DIR})
#link_libraries(libname)

add_library(pugixml SHARED ${SOURCE_FILES})