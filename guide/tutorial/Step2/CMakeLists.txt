cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(Tutorial VERSION 1.0)

# 指定C++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 配置一个头文件来传输CMake的设置到源码
configure_file(TutorialConfig.h.in TutorialConfig.h)

# 添加可执行文件
add_executable(Tutorial tutorial.cxx)

# 为了找到头文件 TutorialConfig.h, 在搜索路径上添加一个二叉树
# 格式target_include_directories(可执行文件名 PUBLIC "${PROJECT_BINARY_DIR}")
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
