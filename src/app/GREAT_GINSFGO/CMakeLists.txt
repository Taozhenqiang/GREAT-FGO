#Cmake 最低版本号要求
cmake_minimum_required(VERSION 3.5.0)

#项目信息
project(${gins})

file(GLOB header_files 	*.h)
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

include_directories(${Third_ceres_ROOT}/include/)
include_directories(${opencv_ROOT}/include/)
include_directories(${GLFW_ROOT}/include/)
include_directories(${pcl_ROOT}/include/pcl-1.9/)
set(PCL_3rdParty    ${pcl_ROOT}/3rdParty) 
include_directories(${PCL_3rdParty}/Boost/include/boost-1_68/)
include_directories(${PCL_3rdParty}/Eigen/eigen3/)
include_directories(${PCL_3rdParty}/FLANN/include/)
include_directories(${PCL_3rdParty}/OpenNI2/Include/)
include_directories(${PCL_3rdParty}/Qhull/include/)
include_directories(${PCL_3rdParty}/VTK/include/vtk-8.1/)

add_definitions(-D GLOG_NO_ABBREVIATED_SEVERITIES)

add_executable(${PROJECT_NAME} ${header_files} ${source_files})

if(CMAKE_SYSTEM_NAME MATCHES "Windows")

	set(link_path
		${ROOT}/build/Lib/Debug
		${ROOT}/build/Lib/Release
		${ROOT}/build/Lib/RelWithDebInfo
		${ROOT}/build/Lib/MinSizeRel
	)
	link_directories(${link_path})
else()
	set(link_path
		${ROOT}/build/Lib)
	link_directories(${link_path})
endif()


set(lib_list 
	${LibGnut}
	${LibGREAT})
	
target_link_libraries(${PROJECT_NAME} ${lib_list})

add_dependencies(${PROJECT_NAME} ${lib_list})
