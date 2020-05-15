cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本号
project(Tutorial VERSION 1.0)

# 指定C++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 设置可以改变值得变量: USE_MYMATH
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# 配置头文件以把版本号传入源码
configure_file(TutorialConfig.h.in TutorialConfig.h)

# 添加 MathFunctions 库
if(USE_MYMATH)
  # 添加子目录
  add_subdirectory(MathFunctions)
  # 设置库变量
  list(APPEND EXTRA_LIBS MathFunctions)
  # 设置头文件目录变量
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# 添加可执行文件
add_executable(Tutorial tutorial.cxx)

# 可执行文件链接到库
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
# 在搜索路径上包含头文件路径
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )
