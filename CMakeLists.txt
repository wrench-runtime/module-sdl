cmake_minimum_required(VERSION 3.6)
set(wrench_module wren_sdl2)
project(module_${wrench_module})

# Add SDL
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
if(WIN32)
set(CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/sdl2/win32/x86)
set(SDL2_BUILDING_LIBRARY on)
#set(SDL2_LIBRARY ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/sdl2/win32/x86/bin/SDL2.dll)
#set(SDL2_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/sdl2/win32/x86/include/SDL2)
#configure_file(${SDL2_LIBRARY} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/../SDL2.dll COPYONLY)
endif()
find_package(SDL2 REQUIRED)

# This is the plugin to be loaded with dlopen
add_library(module_${wrench_module} SHARED ${CMAKE_CURRENT_SOURCE_DIR}/src/${wrench_module}.c)
set_target_properties(module_${wrench_module} PROPERTIES PREFIX "")
set_target_properties(module_${wrench_module} PROPERTIES OUTPUT_NAME "${wrench_module}")
target_link_libraries(module_${wrench_module} PUBLIC wren_runtime ${SDL2_LIBRARY})
target_include_directories(module_${wrench_module} PRIVATE ${SDL2_INCLUDE_DIR})
#message(STATUS ${SDL2_INCLUDE_DIR})

#target_include_directories(module_${wrench_module} PRIVATE ${SDL2_INCLUDE_DIR})

# This is the static module to be included in the runtime host
add_library(module_${wrench_module}_static STATIC ${CMAKE_CURRENT_SOURCE_DIR}/src/${wrench_module}.c)
target_link_libraries(module_${wrench_module}_static PUBLIC wren_runtime ${SDL2_LIBRARY})
target_include_directories(module_${wrench_module}_static PRIVATE ${SDL2_INCLUDE_DIR})

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/wren/${wrench_module}.wren ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${wrench_module}.wren COPYONLY)

