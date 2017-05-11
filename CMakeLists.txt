cmake_minimum_required(VERSION 2.6)

project(clock)

INCLUDE(/usr/share/cmake-2.8/Modules/FindCUDA.cmake)

set(cudaFeatureExtract_VERSION_MAJOR 2)
set(cudaFeatureExtract_VERSION_MINOR 0)
set(cudaFeatureExtract_VERSION_PATCH 0)

set(CPACK_PACKAGE_VERSION_MAJOR "${cudaFeatureExtract_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${cudaFeatureExtract_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${cudaFeatureExtract_VERSION_PATCH}")

include(CPack)

#find_package(OpenCV REQUIRED)
find_package(CUDA)
if (NOT CUDA_FOUND)
  message(STATUS "CUDA not found. Project will not be built.")
endif(NOT CUDA_FOUND)

if (WIN32)
  set(EXTRA_CXX_FLAGS "/DVERBOSE /D_CRT_SECURE_NO_WARNINGS ")
  list(APPEND CUDA_NVCC_FLAGS "-arch=sm_35;--compiler-options;-O2;-DVERBOSE") 
endif()
if (UNIX)
  if (APPLE)
    set(EXTRA_CXX_FLAGS "-DVERBOSE -msse2")
    list(APPEND CUDA_NVCC_FLAGS "-arch=sm_35;--compiler-options;-O2;-DVERBOSE") 
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -msse2 ")
    list(APPEND CUDA_NVCC_FLAGS "-lineinfo;--compiler-options;-O2;-DVERBOSE") 
  endif()
endif()


set(sources
  test.cu

 
)



include_directories(
  /usr/local/cuda/include
/usr/local/cuda-8.0/samples/common/inc
 
)

SET(CUDA_SEPARABLE_COMPILATION ON)

cuda_add_executable(clock ${sources}  OPTIONS -arch=sm_35)



#target_link_libraries(clock
 # /usr/local/cuda/lib64/libcudadevrt.a 
  #/usr/local/cuda/lib64/libcudart.so.8.0
  #/usr/local/cuda/lib64/libcublas.so.8.0
  #/usr/local/cuda/lib64/libcurand.so.8.0
#)






