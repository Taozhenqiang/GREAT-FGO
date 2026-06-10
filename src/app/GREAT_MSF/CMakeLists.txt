#Minimum requirement of CMake version : 3.0.0
cmake_minimum_required(VERSION 3.5.0)

#Project name and version number
project(${msf})

file(GLOB header_files     *.h)
file(GLOB source_files    *.cpp)

source_group("CMake Files" FILES CMakeLists.txt)
source_group("Header Files" FILES header_files)
source_group("Soruce Files" FILES source_files)

set(include_path
    ${Third_Eigen_ROOT}
    ${Third_GLFW_ROOT}/include
    ${LibGnutSrc}
    ${LibGREATSrc})
    
include_directories(${include_path})

add_definitions(-D GLOG_NO_ABBREVIATED_SEVERITIES)

add_executable(${PROJECT_NAME} ${header_files} ${source_files})

if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    set(link_path
        ${BUILD_DIR}/Lib/Debug
        ${BUILD_DIR}/Lib/Release
        ${BUILD_DIR}/Lib/RelWithDebInfo
        ${BUILD_DIR}/Lib/MinSizeRel
    )
    link_directories(${link_path})
else()
    set(link_path
        ${BUILD_DIR}/Lib)
    link_directories(${link_path})
endif()

set(lib_list 
    ${LibGnut}
    ${LibGREAT})
    
target_link_libraries(${PROJECT_NAME} ${lib_list})

add_dependencies(${PROJECT_NAME} ${lib_list})



