cmake_minimum_required(VERSION 3.10)
project(Demo LANGUAGES CXX CUDA)

# 设置 C++ 和 CUDA 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_STANDARD 11)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

# 指定 OpenCV 的库路径
#set(OpenCV_DIR /usr/local/opencv_4.5.5/lib/cmake/opencv4)

# 找到 CUDA 和 OpenCV
find_package(CUDA REQUIRED)
find_package(OpenCV REQUIRED)

# 添加头文件目录
include_directories(${CUDA_INCLUDE_DIRS})
include_directories(${OpenCV_INCLUDE_DIRS})

# 链接 OpenCV 库目录
link_directories(${OpenCV_LIB_DIR})

# 定义可执行文件和源文件
add_executable(demo demo.cu)

# 链接需要的库
target_link_libraries(demo
    ${CUDA_LIBRARIES}
    ${CUDA_CUBLAS_LIBRARIES}
    ${CUDA_curand_LIBRARY}
    ${OpenCV_LIBS} # 自动包含所有 OpenCV 模块
)

# 添加编译选项
target_compile_options(demo PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:-std=c++11 -O3>)
target_compile_definitions(demo PRIVATE -D_MWAITXINTRIN_H_INCLUDED)

# 如果需要排除部分模块，请手动链接特定的库，例如：
# target_link_libraries(demo opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs)

