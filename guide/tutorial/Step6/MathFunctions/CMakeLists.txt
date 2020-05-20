# 生成静态库文件
add_library(MathFunctions mysqrt.cxx)

# 指定库的使用需求为：INTERFACE
# 声明: 任何人链接此库都需要包含当前源码的目录
# 以找到 MathFunctions.h
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

# 检测系统是否提供了 log 和 exp 功能
include(CheckSymbolExists) #添加检测模块
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
# 如果系统不存在 log 或者 exp
if(NOT (HAVE_LOG AND HAVE_EXP))
  unset(HAVE_LOG CACHE)
  unset(HAVE_EXP CACHE)
  set(CMAKE_REQUIRED_LIBRARIES "m")
  check_symbol_exists(log "math.h" HAVE_LOG)
  check_symbol_exists(exp "math.h" HAVE_EXP)
  if(HAVE_LOG AND HAVE_EXP)
	# MathFunctions 私有链接数学库文件 m
    target_link_libraries(MathFunctions PRIVATE m)
  endif()
endif()

# 定义编译规则: 将HAVE_LOG和HAVE_EXP指定为私有编译
if(HAVE_LOG AND HAVE_EXP)
  target_compile_definitions(MathFunctions
                             PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()

# install rules
install(TARGETS MathFunctions DESTINATION lib) # 安装库文件
install(FILES MathFunctions.h DESTINATION include) # 安装库文件的头文件
