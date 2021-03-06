cmake_minimum_required(VERSION 3.3)

if(CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_CONFIGURATION_TYPES Debug Release)
  set(CMAKE_CONFIGURATION_TYPES "${CMAKE_CONFIGURATION_TYPES}" CACHE STRING
    "Set the configurations to what we need" FORCE
    )
endif()


if(CMAKE_COMPILER_IS_GNUCXX)
	set(CMAKE_CXX_FLAGS "-std=c++11 -Ofast -Wall -pedantic -Wextra -Wunused ${CMAKE_CXX_FLAGS}")
	MESSAGE(STATUS "GNU CXX Flags: " ${CMAKE_CXX_FLAGS})	
endif()

project(Hearts)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}) # Findexternpro.cmake
set(externpro_REV 16.10.3)
find_package(externpro REQUIRED)
xpCommonFlags()
xpGetExtern(externIncs externLibs PUBLIC wxWidgets boost)

file(GLOB_RECURSE sources "../src/*.cpp")
file(GLOB_RECURSE test_src "../src/*.cpp" "../test/*.cpp")
list(REMOVE_ITEM test_src "../src/app.cpp")

#add_executable(${PROJECT_NAME} chatClient.cpp chat_message.hpp main.cpp Game.cpp Game.h)
#target_link_libraries(${PROJECT_NAME} ${externLibs})
#target_include_directories(${PROJECT_NAME} ${externIncs})

if(MSVC)
  add_executable(${PROJECT_NAME} WIN32 ${sources})
  target_link_libraries(${PROJECT_NAME} ${externLibs})
  target_include_directories(${PROJECT_NAME} ${externIncs} PUBLIC ../include ../img)
else()
  add_executable(${PROJECT_NAME} ${sources})
  target_link_libraries(${PROJECT_NAME} ${externLibs})
  target_include_directories(${PROJECT_NAME} ${externIncs} PUBLIC ../include ../img)
endif()

add_executable(hTest ${test_src})
target_link_libraries(hTest ${externLibs})
target_include_directories(hTest ${externIncs} PUBLIC lib include test)
