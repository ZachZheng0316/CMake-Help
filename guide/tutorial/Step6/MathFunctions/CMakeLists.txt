# add_library: 主要作用就是将指定的源文件生成链接文件，然后添加到工程中去.
# 从mysqrt.cxx生成链接文件 MathFunctions
add_library(MathFunctions mysqrt.cxx)

# 声明: 任何人链接此库都需要包含当前源码的目录
# 以找到 MathFunctions.h
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

# does this system provide the log and exp functions?
# 检测系统是否提供了 log 和 exp 功能
include(CheckSymbolExists) #添加检测模块
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
if(NOT (HAVE_LOG AND HAVE_EXP))
  unset(HAVE_LOG CACHE)
  unset(HAVE_EXP CACHE)
  set(CMAKE_REQUIRED_LIBRARIES "m")
  check_symbol_exists(log "math.h" HAVE_LOG)
  check_symbol_exists(exp "math.h" HAVE_EXP)
  if(HAVE_LOG AND HAVE_EXP)
    target_link_libraries(MathFunctions PRIVATE m)
  endif()
endif()

# 添加编译定义: 将HAVE_LOG和HAVE_EXP指定为私有编译
if(HAVE_LOG AND HAVE_EXP)
  target_compile_definitions(MathFunctions
                             PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()

# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
