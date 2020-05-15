# 添加库
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 声明：任何人链接此库都需要包含当前目录用以找到 MathFunctions.h
# 但是这个库, 本身不需要当前目录
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )
