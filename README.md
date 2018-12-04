## 简介
>原生态的pugixml，用于处理xml，支持xpath。使用CLion编译成动态库，pugixml的源码编译需要做一些配置，这里做了一些配置。

## 动态库编译
> pugixml默认是静态库，修改成动态库需要做以下修改
* 修改pugixml.hpp
> 添加输出动态库的条件
    * 修改前    
```c++
// If no API is defined, assume default
#ifndef PUGIXML_API
	#define PUGIXML_API
#endif
```
    * 修改后
```c++
// If no API is defined, assume default
#ifndef PUGIXML_API
	#if defined(_MSC_VER) && _MSC_VER >= 1300
		#if defined(PUGI_EXPORT)
			#define PUGIXML_API __declspec(dllexport)
		#else
			#define PUGIXML_API __declspec(dllimport)
		#endif
    #elif (defined(__GNUC__) && ((__GNUC__ > 4) || (__GNUC__ == 4) && (__GNUC_MINOR__ > 2))) || __has_attribute(visibility)
		#if defined(PUGI_EXPORT)
			#define PUGIXML_API __attribute__((visibility("default")))
		#else
			#define PUGIXML_API __attribute__((visibility("default")))
        #endif
    #endif
#else
// If no API is defined, assume default
	#define PUGIXML_API
#endif
```

* 修改CMakeLists.txt
> 增加 ADD_DEFINITIONS(-DPUGI_EXPORT) 以导出dll

## CMakeLists.txt
>设置头文件和lib库文件查找目录，设置输出目录
```
cmake_minimum_required(VERSION 3.12)
project(pugixml)

ADD_DEFINITIONS(-DPUGI_EXPORT)

set(CMAKE_CXX_STANDARD 14)

set(INC_DIR ./ ../include/pugixml)
set(LINK_DIR ../libs/${CMAKE_BUILD_TYPE})
set(SOURCE_FILES pugixml.cpp)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../../libs/${CMAKE_BUILD_TYPE})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ../../libs/${CMAKE_BUILD_TYPE})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../../bin/${CMAKE_BUILD_TYPE})

include_directories(${INC_DIR})
link_directories(${LINK_DIR})

add_library(pugixml SHARED ${SOURCE_FILES})
```