# 生成静态库文件
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 把库 MathFunctions 的头文件设置为 INTERFACE, 使得任何人链接此库都可以找到头文件 MathFunctions.h
# 但是，这个库的本身不需要当前目录
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )
