cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(Tutorial VERSION 1.0)

# 指定C++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 配置一个头文件来传输 CMake 设置到源码
configure_file(TutorialConfig.h.in TutorialConfig.h)

# 生成可执行文件
add_executable(Tutorial tutorial.cxx)

# 为了找到头文件 TutorialConfig.h, 添加 TutorialConfig.h 的头文件
# 按字面意思理解: target 需要包含的文件
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
