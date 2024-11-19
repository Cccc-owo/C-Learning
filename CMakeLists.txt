cmake_minimum_required(VERSION 3.26)

# 获取项目名称
get_filename_component(ProjectId ${CMAKE_CURRENT_SOURCE_DIR} NAME)
string(REPLACE " " "_" ProjectId ${ProjectId})
project(${ProjectId} C)
set(CMAKE_C_STANDARD 17)

# 设置编译器选项以使用 GBK 字符集
if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -finput-charset=UTF-8 -fexec-charset=GBK")
endif()

# 设置编译器选项以使用 GBK 字符集
if(MSVC)
    # 设置代码页为 GBK (936)
    set(ENV{VSCODEPAGE} 936)
endif()

# 递归查找所有 .c 文件，但排除 cmake-build-debug 目录中的文件
file(GLOB_RECURSE files "${CMAKE_CURRENT_SOURCE_DIR}/*.c")
list(FILTER files EXCLUDE REGEX "cmake-build-debug/.*")

# 为每个源文件创建可执行文件
foreach(file ${files})
    get_filename_component(name ${file} NAME_WE)
    add_executable(${name} ${file})
endforeach()
