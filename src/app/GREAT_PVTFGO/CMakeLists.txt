#Cmake 最低版本号要求
cmake_minimum_required(VERSION 3.5.0)

#项目信息
project(${pvtfgo})

file(GLOB header_files 	*.h *.hpp)
file(GLOB source_files	*.cpp)

source_group("CMake Files" FILES CMakeLists.txt)
source_group("Header Files" FILES header_files)
source_group("Soruce Files" FILES source_files)


set(include_path
	${zlib_ROOT}
	${Eigen_ROOT}
	${LibGnutSrc}
    ${LibGREATSrc})
include_directories(${include_path})
include_directories(${GLFW_ROOT}/include/)

add_executable(${PROJECT_NAME} ${header_files} ${source_files})

if(CMAKE_SYSTEM_NAME MATCHES "Windows")
	set(link_path 
		${BUILD_DIR}/Lib/Debug
        ${BUILD_DIR}/Lib/Release
        ${BUILD_DIR}/Lib/RelWithDebInfo
        ${BUILD_DIR}/Lib/MinSizeRel)
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
