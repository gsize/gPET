#####################################################
cmake_minimum_required(VERSION 3.9 FATAL_ERROR)

#####################################################
project(gPET LANGUAGES CXX CUDA)
#set(CMAKE_CUDA_STANDARD 14)
message("project source dir: ${PROJECT_SOURCE_DIR}")

find_package(CUDA REQUIRED)
if (NOT CUDA_FOUND)
	message(status "CUDA not found")
endif (NOT CUDA_FOUND)

IF (WIN32)
	message(STATUS "Now is windows")
	#add_definitions(-Dwin32)
ELSE ()
	message(STATUS "Now is Linux")
	#add_definitions(-DLinux)
	#set(CMAKE_CXX_STANDARD 14)	
	#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")	
	#set(CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")	
	#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")	
	#list(APPEND CMAKE_CXX_FLAGS " -std=c++0x ${CMAKE_CXX_FLAGS} -g -ftest-coverage -fprofile-arcs" )	
ENDIF()


#add include files
include_directories(
${PROJECT_SOURCE_DIR}/
)

message("CUDA_cublas_LIBRARY: ${CUDA_cublas_LIBRARY}")
message("CUDA_curand_LIBRARY: ${CUDA_curand_LIBRARY}")
#LINK_DIRECTORIES(${CUDA_CUBLAS_LIBRARIES})

SET(SRC_gPET ./detector.cu ./gPET.cu ./gPET_kernals.cu ./iniDevice.cu ./initialize.cu ./main.cu )


#generate executable file
add_executable(gPET ${SRC_gPET} )
#target_link_libraries(gPET PUBLIC   ${CUDA_cublas_LIBRARY} ${CUDA_curand_LIBRARY} )
set_target_properties(gPET PROPERTIES CUDA_ARCHITECTURES "35;50;72")
target_link_libraries(gPET PUBLIC   cublas curand)
set_target_properties(gPET PROPERTIES CUDA_SEPARABLE_COMPILATION  ON )

