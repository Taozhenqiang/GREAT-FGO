#Minimum requirement of CMake version : 3.0.0
cmake_minimum_required(VERSION 3.5.0)

#Project name and version number
project(${LibGREAT} VERSION 0.1.0)

# add define
# ==========================================================================================================================================
add_definitions(-D GREAT_LibGREAT_LIBRARY)
add_definitions(-D GLOG_NO_ABBREVIATED_SEVERITIES)

# ==========================================================================================================================================

# link third party
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    #For windows
    # ==============================================================================
    if (NOT Third_Eigen_ROOT OR NOT EXISTS "${Third_Eigen_ROOT}")
        set(Third_Eigen_ROOT       ${ThirdParty_ROOT}/Eigen CACHE PATH "Eigen root directory" FORCE)
    endif()
    if (NOT Third_GLFW_ROOT OR NOT EXISTS "${Third_GLFW_ROOT}")
        set(Third_GLFW_ROOT       ${ThirdParty_ROOT}/GLFW CACHE PATH "GLFW root directory" FORCE)
    endif()
	if (NOT Third_ceres_ROOT OR NOT EXISTS "${Third_ceres_ROOT}")
        set(Third_ceres_ROOT      ${ThirdParty_ROOT}/ceres CACHE PATH "Ceres root directory" FORCE)
    endif()

    # ==============================================================================
else()
    # For linux
    # write write_your_path_here
    # ==============================================================================
    if (NOT Third_Eigen_ROOT OR NOT EXISTS "${Third_Eigen_ROOT}")
        set(Third_Eigen_ROOT       ${ThirdParty_ROOT}/Eigen CACHE PATH "Eigen root directory" FORCE)
    endif()
    if (NOT Third_GLFW_ROOT OR NOT EXISTS "${Third_GLFW_ROOT}")
        set(Third_GLFW_ROOT       ${ThirdParty_ROOT}/GLFW CACHE PATH "GLFW root directory" FORCE)
    endif()
	if (NOT Third_ceres_ROOT OR NOT EXISTS "${Third_ceres_ROOT}")
        set(Third_ceres_ROOT      ${ThirdParty_ROOT}/ceres CACHE PATH "Ceres root directory" FORCE)
    endif()
	
    # ==============================================================================
endif()

if(NOT EXISTS "${Third_ceres_ROOT}/include/ceres/ceres.h")
    message(FATAL_ERROR "Ceres headers were not found at ${Third_ceres_ROOT}/include. Set Third_ceres_ROOT or ceres_ROOT to the Ceres install root.")
endif()

# set include path
include_directories(${Third_Eigen_ROOT})
include_directories(${Third_GLFW_ROOT}/include)
include_directories(${Third_ceres_ROOT}/include)  
include_directories(${LibGnutSrc})
include_directories(${LibGREATSrc})

#message(STATUS "                                                                     ")
message(STATUS " include path for ${LibGPP} is : ${Third_Eigen_ROOT}")
message(STATUS " include path for ${LibGREAT} is : ${LibGREATSrc}")
#message(STATUS "                                                                     ")

# for all the header files and source_files
file(GLOB_RECURSE header_files ${PROJECT_SOURCE_DIR} *.h *.hpp)
file(GLOB_RECURSE source_files ${PROJECT_SOURCE_DIR} *.cpp)
file(GLOB_RECURSE header_files ${PROJECT_SOURCE_DIR} *.h *.hpp)
file(GLOB_RECURSE source_files ${PROJECT_SOURCE_DIR} *.cpp)

# grouping
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    # add files to project
    source_group("CMake Files"  FILES CMakeLists.txt)
    source_group("Header Files" FILES header_files)
    source_group("Soruce Files" FILES source_files)

    # find files and put them into different groups
    foreach(fold ${file_fold})  
    file(GLOB_RECURSE tmp_header_files 
        ${PROJECT_SOURCE_DIR}/${fold}/*.h 
        ${PROJECT_SOURCE_DIR}/${fold}/*.hpp)
    file(GLOB_RECURSE tmp_source_files 
        ${PROJECT_SOURCE_DIR}/${fold}/*.cpp)
    source_group("Header Files\\${fold}"        FILES ${tmp_header_files})
    source_group("Source Files\\${fold}"        FILES ${tmp_source_files})
endforeach(fold)      
else()
    #message(STATUS "For Linux / unix, there is no need to group so detailed")
endif()

# grouping in VS
# ==========================================================================================================================================
if(${PrintFile})
    message(STATUS " ====================================================================================================================== ")
    message(STATUS " header_files_for_${PROJECT_NAME}" )
    foreach(list ${header_files})   
        message(STATUS " ==> ${list}")
    endforeach(list)   

    message(STATUS " source_files_for_${PROJECT_NAME}" )
    foreach(list ${source_files})   
        message(STATUS " ==> ${list}")
    endforeach(list)   
    message(STATUS " ====================================================================================================================== ")
endif()

# link project
# ==========================================================================================================================================
add_library(${PROJECT_NAME} SHARED ${header_files} ${source_files})
# ==========================================================================================================================================

# link third
# ==========================================================================================================================================
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    link_directories(${BUILD_DIR}/Lib/Debug)
    link_directories(${BUILD_DIR}/Lib/Release)
    link_directories(${BUILD_DIR}/Lib/RelWithDebInfo)
    link_directories(${BUILD_DIR}/Lib/MinSizeRel)
	link_directories(${BUILD_DIR}/Lib)
	target_link_libraries(${PROJECT_NAME} ${Third_GLFW_ROOT}/lib/glfw3.lib opengl32.lib)
	target_link_libraries(${PROJECT_NAME}			
		optimized ${Third_GLFW_ROOT}/lib/glfw3.lib	
		${Third_ceres_ROOT}/lib/ceres.lib
		${Third_ceres_ROOT}/lib/gflags.lib
		${Third_ceres_ROOT}/lib/glog.lib
		${Third_ceres_ROOT}/lib/lapack.lib
		${Third_ceres_ROOT}/lib/libamd.lib
		${Third_ceres_ROOT}/lib/libbtf.lib
		${Third_ceres_ROOT}/lib/libcamd.lib
		${Third_ceres_ROOT}/lib/libccolamd.lib
		${Third_ceres_ROOT}/lib/libcholmod.lib
		${Third_ceres_ROOT}/lib/libcolamd.lib
		${Third_ceres_ROOT}/lib/libcxsparse.lib
		${Third_ceres_ROOT}/lib/libklu.lib
		${Third_ceres_ROOT}/lib/libldl.lib		
		${Third_ceres_ROOT}/lib/libspqr.lib
		${Third_ceres_ROOT}/lib/libumfpack.lib
		${Third_ceres_ROOT}/lib/openblas.lib
		${Third_ceres_ROOT}/lib/suitesparseconfig.lib	
		)
	
set(FGO_dllfile
		${Third_ceres_ROOT}/bin/ceres.dll
		${Third_ceres_ROOT}/bin/gflags.dll
		${Third_ceres_ROOT}/bin/glog.dll
		${Third_ceres_ROOT}/bin/lapack.dll
		${Third_ceres_ROOT}/bin/openblas.dll)
	file(COPY 	${FGO_dllfile}	DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Release			)
	file(COPY 	${FGO_dllfile}	DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/MinSizeRel		)
	file(COPY 	${FGO_dllfile}	DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/RelWithDebInfo	)
	
else()		
	# linux need todo
	link_directories(${ROOT}/build/Lib)
	if(Main_USE_EXELIB)
		target_link_libraries(${PROJECT_NAME}	debug	${ExeLib}/libLibBased.so	optimized	${ExeLib}/libLibBase.so	)
		target_link_libraries(${PROJECT_NAME}	debug	${ExeLib}/libLibSetd.so		optimized	${ExeLib}/libLibSet.so	)
		target_link_libraries(${PROJECT_NAME}	debug	${ExeLib}/libLibGnutd.so	optimized	${ExeLib}/libLibGnut.so	)
	endif()
endif()
# ==========================================================================================================================================
target_link_libraries(${PROJECT_NAME}    ${LibGnut})
add_dependencies(${PROJECT_NAME}       ${LibGnut})

set(pvt     GREAT_PVT)
add_subdirectory(${ROOT}/app/${pvt}    ${BUILD_DIR}/${pvt})
SET_PROPERTY(TARGET ${pvt}     PROPERTY FOLDER "app")

set(msf       GREAT_MSF)
add_subdirectory(${ROOT}/app/${msf}   ${BUILD_DIR}/${msf})
SET_PROPERTY(TARGET ${msf}    PROPERTY FOLDER "app")

set(pvtfgo       GREAT_PVTFGO)
add_subdirectory(${ROOT}/app/${pvtfgo}   ${BUILD_DIR}/${pvtfgo})
SET_PROPERTY(TARGET ${pvtfgo}    PROPERTY FOLDER "app")


set(gins       GREAT_GINSFGO)
add_subdirectory(${ROOT}/app/${gins}   ${BUILD_DIR}/${gins})
SET_PROPERTY(TARGET ${gins}    PROPERTY FOLDER "app")
