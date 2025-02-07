cmake_minimum_required (VERSION 3.9)
project (fusibile VERSION)

# Enable C++11 globally
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Support IDEs: https://cliutils.gitlab.io/modern-cmake/chapters/features/ides.html
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(GLOBAL PROPERTY PREDEFINED_TARGETS_FOLDER "cmake-default-targets")

find_package(OpenCV REQUIRED)
find_package(CUDA 12.2 REQUIRED ) # change to your cuda version, my is 12.2 in /usr/local path

if(NOT DEFINED CMAKE_CUDA_STANDARD)
    set(CMAKE_CUDA_STANDARD 17)
    set(CMAKE_CUDA_STANDARD_REQUIRED ON)
endif()

include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(opencv/build/include)

# from https://en.wikipedia.org/wiki/CUDA#GPUs_supported
set(CMAKE_CUDA_ARCHITECTURES "native")
#set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-O3 --use_fast_math --ptxas-options=-v -std=c++17 --compiler-options -Wall -gencode arch=compute_52,code=sm_52)

if(CMAKE_COMPILER_IS_GNUCXX)
    add_definitions(-std=c++17)
    add_definitions(-Wall)
    add_definitions(-Wextra)
    add_definitions(-pedantic)
    add_definitions(-Wno-unused-function)
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3 -ffast-math -march=native") # extend release-profile with fast-math
    #add_definitions(-march=native)
endif()


# for YouCompleteMe
set( CMAKE_EXPORT_COMPILE_COMMANDS 1 )

# For compilation ...
# Specify target & source files to compile it from
cuda_add_executable(
    fusibile
    cameraGeometryUtils.h
    vector_operations.h
    camera.h
    globalstate.h
    algorithmparameters.h
    cameraparameters.h
    linestate.h
    displayUtils.h
    mathUtils.h
    fileIoUtils.h
    fusibile.cu
    main.cpp
    )
# https://cliutils.gitlab.io/modern-cmake/chapters/packages/OpenMP.html
find_package(OpenMP)

message(${OpenCV_LIBS})

#add the opencv lib path
link_directories("/usr/local/lib")
# For linking ...
# Specify target & libraries to link it with
target_link_libraries(fusibile
	${OpenCV_LIBS}
	OpenMP::OpenMP_CXX
	)


include(FeatureSummary)
feature_summary(WHAT ALL)
